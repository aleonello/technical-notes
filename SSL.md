# SSL Certificate

## Links

* [Error "Alias name[certificate_alias_name] does not identify a key entry" during HTTPS setup](https://support.tibco.com/s/article/Error-Alias-name-certificate-alias-name-does-not-identify-a-key-entry-during-HTTPS-setup)

```sh
openssl pkcs12 -export -in fullchain.pem -inkey privkey.pem -out domain.pfx
```

## How generate CSR

```sh
openssl req -new -newkey rsa:2048 -nodes -keyout [your_domain].key -out your_domain.csr
```

## How convert CA to K8S TLS

1. Combine the Certificate and CA Bundle:
Combine your signed certificate and CA bundle into a single file if your application expects the full chain:

```sh
cat certificate.crt ca-bundle.crt > fullchain.crt
```
example:
```sh
cat STAR_example_com.crt STAR_example_com.srl > fullchain.crt
```

2. Create a Kubernetes Secret:
Kubernetes expects the certificate and key to be base64 encoded. We will create a secret containing these.
First, base64 encode the certificate and the private key (where is generated by CSR step)

```sh
base64 -w 0 fullchain.crt > cert_base64.txt
base64 -w 0 private.key > key_base64.txt
```

## Create SSH certificate and add to a remote host

1. Execute the following command in the local machine:

```sh
ssh-keygen -t rsa -b 2048
cat ~/.ssh/id_rsa.pub
```
2. Get re result of the previous command and fill in the `your-public-key-content` below to run in the remote host:

```sh
echo "your-public-key-content" >> ~/.ssh/authorized_keys
```

3. Conect to the remote host using the command below in your local machine:

```sh
ssh  root@bastion.domain.com
```

## Check certificates with Certbot

```sh
sudo certbot certificates
```

## Renew a certificate with Certbot

```sh
sudo certbot renew
```
