# Practise enbale SSL for your nginx server

## docker run nginx
`docker run -it --rm -d -p 8080:80 --name web nginx`
 
 command options:
 1. -it : start a interactive terminal
 2. -d : run your container as a background process
 3. -p : map your local port to port in container
 4. -rm : delete container when container stoped/terminated

 open un-secured connection by -> curl http://localhost:8080

## Enable SSL on Nginx

1. Submit the CSR to a Certificate Authority and download the certificate with its private key

Certificat Authority (CA) is one of the main entities in PKI (public Key infrustructure, it is a set of hardware/software/encryption-technologies and service that manage digital certificates), whose responsibility is to issue, revoke, and maintain the database of digital certificates that are used to provide the identity of the object over public/private network. the primary role of the CA is to digitally sign and publish a public key (certificate that contains public key)

``` sh
docker exec -it XXX /bin/sh

apt upgrade

mkdir /root/ca
mkdir /root/ca/certs
mkdir /root/ca/crl
mkdir /root/ca/newcerts
mkdir /root/ca/private
mkdir /root/ca/requests
touch /root/ca/index.txt

echo '1000' > /root/ca/serial
chmod 600 /root/ca
cd /root/ca

# Run this command to create an RSA private key cakey.pem of length 4096 bits for CA certificate
openssl genrsa -aes256 -out private/cakey.pem 4096 

# Create a public certificate for CA using the private key
openssl req -new -x509 -key /root/ca/private/cakey.pem -out cacert.pem -days 3650

# update CA path in /etc/ssl/openssl.cnf
dir = /root/ca
...

# now CA is ready to issue certificate, you can create a certificate signing request
cd requests
openssl req -new -newkey rsa:2048 -nodes -keyout localhost.key -out localhost.csr

# Command to issue the certificate
openssl ca -in localhost.csr -out localhost.crt

# edit your default nginx conf to disable listener 80 and add 443 instead
# conf file under /etc/nginx/conf.d/default.conf
```
server {
   
    listen 443 ssl;
    server_name  localhost;
    ssl_certificate /root/ca/requests/localhost.crt;
    ssl_certificate_key /root/ca/requests/localhost.key;

    location / {
        root   /usr/share/nginx/html;
        index  index.html index.htm;
    }

   
}


```