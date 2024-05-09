# SSL Certificate

## Links

* [Error "Alias name[certificate_alias_name] does not identify a key entry" during HTTPS setup](https://support.tibco.com/s/article/Error-Alias-name-certificate-alias-name-does-not-identify-a-key-entry-during-HTTPS-setup)

```sh
openssl pkcs12 -export -in fullchain.pem -inkey privkey.pem -out domain.pfx
```
