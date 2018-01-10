---
title: Troubleshooting UCP certificate's subject alternative names (SAN)
internal: no
comment: "OpenSSL troubleshooting commands for UCP SANs"
type: kbase
author: aathomas
product:
  - ee
platform:
  - linux
tags:
  - error
  - security
---
## Issue

After implementing third party TLS certificates, you receive a message complaining about failing to connect to UCP:

```
> FATA[0000] failed to get new conv client: failed to create ucp client from ucp opts: Failed to connect to UCP; make sure that you are using a domain listed in UCP's TLS certificate's subject alternate names: Get https://192.168.0.152/_ping: x509: cannot validate certificate for 192.168.0.152 because it doesn't contain any IP SANs
```

## Prerequisites

Before performing these steps, you must meet the following requirements:

- Ensure openSSL packages are installed. For example, for rpm-based systems:

    ```bash
    # yum install openssl openssl-devel openssl-libs -y
    ```

## Resolution

The openssl program is a command line tool for using the various cryptography functions of OpenSSL's crypto library from the shell. You can use this tool to verify a certificate's SAN or if a private key matches a certificate.

1. Check your third party TLS certificates for subject alternative names (SAN) in a container formatted pem file commonly used with UCP:

    ```bash
    # openssl x509 -text -noout -in server-cert.pem | grep "X509v3 Subject Alternative Name" -A1 
      X509v3 Subject Alternative Name: 
      DNS:*.example.com, IP Address:127.0.0.1
    ```
    
2. Check your original UCP certificates for the correct SAN names assuming these certificates were properly working. These will usually contain other UCP managers or a UCP load balancer's name. 

    ```bash
    openssl x509 -text -noout -in server-original.pem | grep "X509v3 Subject Alternative Name" -A1
      X509v3 Subject Alternative Name:
      DNS:*.example.com, IP Address:192.168.0.152, IP Address:192.168.0.154, IP Address:192.168.0.158, IP Address:127.0.0.1
    ```
3. Create new certificates including these subject alternative names, including the correct IP addresses obtained from the original UCP certificates similar to the example process below:

    ```bash
    # openssl genrsa -aes256 -out ca-key.pem 4096 
    # openssl genrsa -out server-key.pem 4096 
    # openssl req -subj "/CN=*.example.com" -sha256 -new -key server-key.pem -out server.csr 
    ```

    Here is where you want to include the correct DNS/IP addresses as SANs.
 
    ```bash
    # echo subjectAltName = DNS:*.example.com,IP:192.168.0.152,IP:192.168.0.154,IP:192.168.0.158,IP:127.0.0.1 >> extfile.cnf 
    # echo extendedKeyUsage = clientAuth >> extfile.cnf 
    # openssl x509 -req -days 365 -sha256 -in server.csr -CA ca.pem -CAkey ca-key.pem -CAcreateserial -out server-cert.pem -extfile extfile.cnf
    ```

     

4. *Additional troubleshooting:* Ensure a private key matches a certificate for a ca and server key pairs using openssl md5 validation checksums: Enter passphrase generated in the previous steps if prompted: 

    Checking the ca key pairs:
    ```bash
    # openssl x509 -noout -modulus -in ca.pem | openssl md5 
    (stdin)= 0257581b2675ca80050e67d34d93a585
    # openssl rsa -noout -modulus -in ca-key.pem | openssl md5 
    (stdin)= 0257581b2675ca80050e67d34d93a585
    ``` 
    Checking the server key pairs
    ```bash
    # openssl x509 -noout -modulus -in server-cert.pem | openssl md5 
    (stdin)= 226c08030a31cda4584f5ffd34610c51
    # openssl rsa -noout -modulus -in server-key.pem | openssl md5
    (stdin)= 226c08030a31cda4584f5ffd34610c51
    ```


### What's Next

- [Creating ca-server client keys with openssl](https://docs.docker.com/engine/security/https/#create-a-ca-server-and-client-keys-with-openssl)
