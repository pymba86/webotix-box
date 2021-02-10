# webotix-box

### Prerequisites

- Your own domain and the understanding of how to configure it to point to your server. Alternative, an account with https://www.noip.com/ and a domain configured there (we'll set it up so that the domain is automatically updated).
- A suitable Linux-based host with:
    - No unnecessary services (such as `httpd` or `sambdad`)
    - A suitable update strategy to ensure security patches are installed quickly
    - `sshd` secured and locked down. There are many guides out there. [Here's one](http://acmeextension.com/secur-ssh-server/).
    - Docker and docker-compose installed.

### Steps

- SSH into your server.
- `su` to get a root prompt.
- `mkdir /opt/webotix`
- `cd /opt/webotix`
- `git clone https://gitlab.com/PyMba86/webotix-box.git`
- Modify `data/nginx/app.conf`, replacing the two instances of `yourserverhere.example.com` with your domain.
- Now test setting up your SSL certificate: `./init-letsencrypt.sh your.domain.com your@email.com 1`
- If that worked OK, you can do it for real: `./init-letsencrypt.sh your.domain.com your@email.com 0`
- `chmod +x generate-secrets.sh`
- Generate a clean set of unused credentials: `./generate-secrets.sh`
- Note down the credentials printed at the end. Some of these will not be shown again.
- Now add your secrets, replacing XXX with the relevant values and skipping any features you don't use:

```
echo -n "XXX" > "data/secret/secret-telegram-botToken"
echo -n "XXX" > "data/secret/secret-telegram-chatId"
echo -n "XXX" > "data/secret/secret-exchanges-binance-apiKey"
echo -n "XXX" > "data/secret/secret-exchanges-binance-secretKey"
```

Optional 

```
echo -n "XXX" > "data/secret/secret-script-signing-key"
echo -n "XXX" > "data/secret/secret-jwt-signing-key"
echo -n "XXX" > "data/secret/secret-jwt-username"
echo -n "XXX" > "data/secret/secret-jwt-salt"
echo -n "XXX" > "data/secret/secret-jwt-password"
```

- Lock down that directory:

```
chmod -R 600 data/secret
chmod 700 data/secret
```

To start, use:

`docker-compose up -d`
