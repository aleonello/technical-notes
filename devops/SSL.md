# SSL Certificate

## Links

* [Error "Alias name does not identify a key entry" during HTTPS setup](https://support.tibco.com/s/article/Error-Alias-name-certificate-alias-name-does-not-identify-a-key-entry-during-HTTPS-setup)
* [Let's Encrypt Documentation](https://letsencrypt.org/docs/)
* [Certbot Documentation](https://certbot.eff.org/docs/)
* [OpenSSL Man Page](https://www.openssl.org/docs/manmaster/man1/openssl.html)
* [Mozilla SSL Configuration Generator](https://ssl-config.mozilla.org/)
* [SSL Labs SSL Server Test](https://www.ssllabs.com/ssltest/)

## Export certificate to PKCS12 format

```sh
openssl pkcs12 -export -in fullchain.pem -inkey privkey.pem -out domain.pfx
```

## How to generate a CSR

```sh
openssl req -new -newkey rsa:2048 -nodes -keyout [your_domain].key -out your_domain.csr
```

## How to convert a CA certificate to a K8S TLS secret

1. Combine the certificate and CA bundle into a full chain:

```sh
cat certificate.crt ca-bundle.crt > fullchain.crt
```

Example:
```sh
cat STAR_example_com.crt STAR_example_com.srl > fullchain.crt
```

2. Base64-encode the certificate and private key (private key is generated during the CSR step):

```sh
base64 -w 0 fullchain.crt > cert_base64.txt
base64 -w 0 private.key > key_base64.txt
```

## Create SSH certificate and add to a remote host

1. Generate the key pair on the local machine:

```sh
ssh-keygen -t rsa -b 2048
cat ~/.ssh/id_rsa.pub
```

2. Add the public key to the remote host's authorized keys:

```sh
echo "your-public-key-content" >> ~/.ssh/authorized_keys
```

3. Connect to the remote host:

```sh
ssh root@bastion.domain.com
```

## Check certificates with Certbot

```sh
sudo certbot certificates
```

## Renew a certificate with Certbot

```sh
sudo certbot renew
```
