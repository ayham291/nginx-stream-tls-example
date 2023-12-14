# Setting up NGINX as entry point

This configuration allows access to a container and to the host.

## WETTY - Web TTY

Wetty is a terminal over HTTP and HTTPS. It's built with Node.JS, and it runs
on the server, allowing you to access servers from a web browser.

## NGINX - TCP Proxy

Allows access to a host with ssh directly from terminal with ssh.

## Adding client certificate to the browser

Using your certificate generated by the CA, you can add it to the browser and
access the server without the need to enter the password.

The key and the certificate/certificate chain must be in the same file.

```bash
openssl pkcs12 -export -out client.pfx -inkey client.key -in client.crt
```

Additionally you will need to add the CA certificate and all intermediate
certificates to the browser.

## Generating CA and certificates

Using [pki](https://github.com/ayham291/pki) you can generate the CA and
certificates.

Here is an example of how to generate the CA and intermediate certificates.

```bash
myPKI -t root -d <DAYS>
myPKI -t network -d <DAYS>
myPKI -t component -d <DAYS>
```

Here is an example of how to generate the server certificate.

```bash
myPKI -s server -d <DAYS> -n <HOSTNAME|FQDN>
myPKI -s client -d <DAYS> -n <NMAE>
```

### NGINX configuration

In order to try it out you will need add the certificate fingerprint 
to the NGINX configuration.

```bash
openssl x509 -in client.crt -noout -sha256 -fingerprint
```