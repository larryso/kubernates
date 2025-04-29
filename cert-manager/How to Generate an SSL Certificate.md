# How to Generate an SSL Certificate

Generating an SSL certificate depends on whether you need a self-signed certificate (for testing/development) or a trusted certificate from a Certificate Authority(CA) for PROD use.

## Option 1: self-signed certificate
1. Generate a private key and certificate
   `openssl req -x509 -newkey rsa:4096 -keyout key.pem -out cert.pem -days 365 -nodes`

2. Answer the prompts (You can leave most blank except Common Name which should match your domain or localhost for testing)

## Option2: Trusted Certificate from a CA
Using Let's Encrypt (free certificate)

1. Installing certbot
```
sudo apt-get install software-properties-common
sudo add-apt-repository ppa:certbot/certbot
sudo apt-get update
sudo apt-get install certbot
```
3. Creating certificates
```
certbot certonly --manual --preferred-challenges=dns --key-size 4096 -d mydomain.com -d www.mydomain.com

# COMMAND BREAKDOWN
# --manual: Indicates you want to handle the DNS challenge manually.
# --preferred-challenges=dns: Tells Certbot to use the DNS challenge method.
# --key-size 4096: Specifies that you want to use a 4096-bit RSA key for better security.
# -d mydomain.com -d www.mydomain.com: This includes both the root domain mydomain.com and subdomain www.mydomain.com to ensure that both are covered by the certificate.
# you can give a wildcard domain to this if needed.
```
This command will prompt you to provide an email address for notifications. It will then generate a TXT record that needs to be added to your DNS provider for certbot to verify domain ownership.

Verifying the domain ownership

Certbot will output specific DNS records (TXT records) that you need to add to your DNS provider to complete the DNS verification process. Following is a sample output,

```
- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
Please deploy a DNS TXT record under the name:

_acme-challenge.mydomain.com.

with the following value:

5lBTUVMngGj346hseQgIqAE29JusY76g2moq2gnETVBuw

- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
```
Once you’ve added the required DNS records, Certbot will verify domain ownership and issue the certificate. It may take a few minutes for the DNS to update the TXT records.

If everything is successful, certbot will display the locations where the certificates are saved locally.

```
Successfully received certificate.
Certificate is saved at: /etc/letsencrypt/live/mydomain.com/fullchain.pem
Key is saved at:         /etc/letsencrypt/live/mydomain.com/privkey.pem
This certificate expires on 2024-12-19.
These files will be updated when the certificate renews.
```
Please note that these certificates are valid for only 90 days, and since they were created using the manual plugin, auto-renewal is not enabled. You will need to renew them manually.
