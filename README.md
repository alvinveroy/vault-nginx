# Vault on Nginx Reverse Proxy

### Prerequisites
1. Docker
2. Docker Compose
3. Domain name (for ssl and vault accessibility)
4. Cloudflare account
5. Cloud Service or VPS (Virtual Private Server) such as AWS, Azure, Digitalocean

### Installation
1. In your server console clone this repository 
```
$ git clone https://github.com/alvinveroy/vault-nginx.git
```
2. Sign up for [cloudflare](https://www.cloudflare.com) 
3. Add an A record for your Vault's subdomain and enter your VPS or Cloud VM IP. Make sure to enable proxy so that your Vault's server IP will be hidden to anyone who will dig your DNS records.
![ca6b3e81.png](https://raw.githubusercontent.com/alvinveroy/vault-nginx/master/documentation_images/ca6b3e81.png)
4. Obtain the free 15 years SSL Certificate from cloudflare. 
![ac37e31d.png](https://raw.githubusercontent.com/alvinveroy/vault-nginx/master/documentation_images/ac37e31d.png)
5. Click on the create button. 
![9f2b246a.png](https://raw.githubusercontent.com/alvinveroy/vault-nginx/master/documentation_images/9f2b246a.png)
6. Create a file under the folder ssl and name is as "ssl.crt" - paste the content of Origin certificate that you have copied from cloudflare, then create another file and name it "ssl.key". Paste the contents of the private key from cloudflare inside ssl.key file. MAKE SURE NOT TO CLICK THE OK BUTTON IF YOU HAVEN'T SAVE THE CONTENTS OF THE PRIVATE KEY. You will not able to get it again and must revoke your Certificate and create a new one.
![8565a555.png](https://raw.githubusercontent.com/alvinveroy/vault-nginx/master/documentation_images/8565a555.png)
7. Open the file nginx.conf under nginx_conf folder and edit the server_name to your Vault's domain name. Example my domain name is "alvin.tech" with subdomain "vault".
![55727702.png](https://raw.githubusercontent.com/alvinveroy/vault-nginx/master/documentation_images/55727702.png)

**You are now ready to launch your Vault cluster.**

8. In your server console go inside the director named vault-nginx and type docker network create vault-network then docker-compose up -d
```
$ cd vault-nginx
$ docker network create vault-network
$ docker-compose up -d
```
### Vault Initialization

1. Go to your Vault's web ui https://vault.<your_domain_name>/ui You will be asked to initialize your vault with number of Key Shares and Key Threshold. This will create unseal keys and one to keep for yourself and others to be distributed to key personel. It's avisable to always have tow or more people to  unseal the vault in an event that it was restarted. I recommend creating 3 key shares and 2 key threshold. You may distribute two keys to two personel that way in an event that one person is not available to unseal the vault with you, theres always one backup who could help you start it.
![0f3d4e43.png](https://raw.githubusercontent.com/alvinveroy/vault-nginx/master/documentation_images/0f3d4e43.png)

2. Download the keys it contains root token and I strongly suggest to put it in a safe container. I suggest using https://www.keybase.io and encrypt the token and your unseal key. After distributing the unseal keys delete the file. DO NOT KEEP ALL THE UNSEAL KEYS BY YOURSELF.
![d3461560.png](https://raw.githubusercontent.com/alvinveroy/vault-nginx/master/documentation_images/d3461560.png)


### Congratulations you now have Hashicorp Vault running on your server in a production mode.

You may now store all your sensitive credentials like database URI's and password and let Vault rotate the credentials for you. There are lot of tutorials online on how to use it and it will open you to the world of confidential computing.