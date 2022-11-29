Deploy Explorer

1. Clone Repo
```
cd ~
git clone https://github.com/ping-pub/explorer
```

2. Edit Config
```
nano ~/explorer/src/chains/mainnet/<CHAIN_NAME>.json
```


```
{
    "chain_name": "XXX",
    "coingecko": "",
    "api": ["https://Your API"],
    "rpc": ["https://Your RPC"],
    "sdk_version": "0.42.4",
    "addr_prefix": "neutron",
    "logo": "/logos/neutron.jpg",
    "assets": [{
        "base": "untrn",
        "symbol": "NTRN",
        "exponent": "6",
        "coingecko_id": "",
        "logo": "/logos/neutron.jpg"
    }]
}
```

3. Edit Config Domain
```
wget  https://raw.githubusercontent.com/ping-pub/explorer/master/ping.conf -O /etc/nginx/sites-enabled/ping.con
```

```
nano /etc/nginx/sites-enabled/ping.conf
```

```
server {
    listen       80;
    listen  [::]:80;
    server_name  <GANTI_DOMAIN_EXPLORER>;

    #access_log  /var/log/nginx/host.access.log  main;

    location / {
        root /usr/share/nginx/html;
        index  index.html index.htm;
        try_files $uri $uri/ /index.html;
    }

    #error_page  404              /404.html;

    # redirect server error pages to the static page /50x.html
    #
    error_page   500 502 503 504  /50x.html;
    location = /50x.html {
        root   /usr/share/nginx/html;
    }

    gzip on;
    gzip_proxied any;
    gzip_static on;
    gzip_min_length 1024;
    gzip_buffers 4 16k;
    gzip_comp_level 2;
    gzip_types text/plain application/javascript application/x-javascript text/css application/xml text/javascript application/x-httpd-php application/vnd.ms-fontobject font/ttf font/opentype font/x-woff image/svg+xml;
    gzip_vary off;
    gzip_disable "MSIE [1-6]\.";
}
```

```
sed -i 's/usr\/share\/nginx/var\/www/g' /etc/nginx/sites-enabled/ping.conf
```

4. Build
```
cd ~/explorer
yarn && yarn build
```

5. Update 
```
curl -fsSL https://deb.nodesource.com/setup_16.x | sudo -E bash -
sudo apt update &&  apt-get install -y nodejs curl git
```

```
curl -sL https://dl.yarnpkg.com/debian/pubkey.gpg | gpg --dearmor | sudo tee /usr/share/keyrings/yarnkey.gpg >/dev/null
echo "deb [signed-by=/usr/share/keyrings/yarnkey.gpg] https://dl.yarnpkg.com/debian stable main" | sudo tee /etc/apt/sources.list.d/yarn.list
sudo apt-get update && sudo apt-get install yarn -y
```

6. Install SSL
```
certbot --nginx --redirect -d YOUR EXPLORER DOMAIN
```

6. Hosting
```
cp -r $HOME/explorer/dist/* /usr/share/nginx/html
sudo systemctl restart nginx
```
