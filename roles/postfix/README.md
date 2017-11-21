# Postfix

## Generate DKIM key

Generate a new file with openssl and put it into `files/domainkey.pem`:

```bash
openssl genrsa -out domainkey.pem 1024 -outform PEM
```

This will produce private key in domainkey.pem file.

Take the public key from it:

```bash
openssl rsa -in domainkey.pem -out domainkey-public.pem -pubout -outform PEM
```

This will produce public key in domainkey-public.pem file.

## DKIM DNS record

You have to create TXT record for `._domainkey` subdomain. This TXT record must contain the public key, i.e. the content of the domainkey-public.pem file without initial `-----BEGIN PUBLIC KEY-----` and final `-----END PUBLIC KEY-----` and without new line and space characters. Also the TXT record must have `v=DKIM1; k=rsa; p=` prefix.

For example:

```bash
dig txt mail._domainkey.example.com

# mail._domainkey.example.com. 1800 IN TXT    "v=DKIM1; k=rsa; p=MIGfMA0GCSqGSIb3DQEBAQUAA4GNADCBiQKBgQCznDnaK7u9i4rjL/e8oW4GqLk04YdhigZOHheR7FmofSRe/KE3Tv7r+mEMeR4IkonK9SIswkeLBVYYsiIAnbClA207ANf7zr9u5Ca1QjlsWBxWhSRVHKJOgoF3yv19o7/lHeAe8FzAaMqSeEhp9RG9NDUIxXuUhrzHCLbBhMLZrQIDAQAB"
```