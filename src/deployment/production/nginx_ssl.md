# nginx and SSL certificates

When we start the application, everything will simply be accessible from localhost. However, localhost is not accessible outside the server. Instead, we use a so-called "reverse proxy" to allow access to the application. This reverse proxy also ensures we use can TLS (which is so you can have that important https link, which means all communication is encrypted). 

## nginx configuration

The configuration for nginx is stored in `/etc/nginx/sites-available`:


```nginx
server {

	root /var/www/api.dsavdodeka.nl/html;

    index index.html index.htm index.nginx-debian.html;

    server_name api.dsavdodeka.nl;

    location / {
            # First attempt to serve request as file, then
            # as directory, then fall back to displaying a 404.
            proxy_pass http://localhost:4241;
            proxy_http_version 1.1;
            proxy_set_header Upgrade $http_upgrade;
            proxy_set_header Connection 'upgrade';
            proxy_set_header Host $host;
            proxy_cache_bypass $http_upgrade;
    }

    listen [::]:443 ssl ipv6only=on; # managed by Certbot
    listen 443 ssl; # managed by Certbot
    ssl_certificate /etc/letsencrypt/live/api.dsavdodeka.nl/fullchain.pem; # managed by Certbot
    ssl_certificate_key /etc/letsencrypt/live/api.dsavdodeka.nl/privkey.pem; # managed by Certbot
    include /etc/letsencrypt/options-ssl-nginx.conf; # managed by Certbot
    ssl_dhparam /etc/letsencrypt/ssl-dhparams.pem; # managed by Certbot

}

server {
    if ($host = api.dsavdodeka.nl) {
        return 301 https://$host$request_uri;
    } # managed by Certbot


	listen 80;
	listen [::]:80;

    server_name api.dsavdodeka.nl;
    return 404; # managed by Certbot
}
```

Everything that says "`# managed by Certbot`" we can ignore. The important part is the `location` block. It basically says to forward http://localhost:4241, which is the URL of our application, to the outside world at port 80 (and 443 for TLS). The `server_name` is also important, because this tells nginx to only forward it if these requests come from `api.dsavdodeka.nl`. The rest is all unchanged from the defaults.

The file in `sites-available` is not the one actually read, instead it reads from `sites-enabled`. The latter is actually linked so that it automatically updates based on the former. This can be achieved using a command like:

```shell
sudo ln -s /etc/nginx/sites-available/api.dsavdodeka.nl /etc/nginx/sites-enabled/api.dsavdodeka.nl
```

## Certbot (Let's Encrypt)

TLS certificates are necessary to have an encrypted https URL. These are given out by special providers. We use the Cerbot tool to get one from Let's Encrypt. First the site must be available at a normal http link on port 80. Then, after running Certbot (see [instructions](https://certbot.eff.org/instructions?ws=nginx&os=ubuntufocal)), it makes sure you can use https and modifies the nginx configuration to make that happen.