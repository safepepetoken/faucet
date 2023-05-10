ssh root@154.49.246.64
Pepesafe123@

apt-get update
apt-get install git

ssh-keygen
cat ~/.ssh/id_rsa.pub

git clone git@github.com:safepepetoken/faucet.git

curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.37.2/install.sh | bash
. ~/.nvm/nvm.sh
nvm install 16
nvm alias default 16
node -v
npm i -g pm2
npm install -g yarn

cd faucet/ && yarn install && yarn build && yarn start_prod && cd ../

nano /etc/nginx/conf.d/faucet-pepesafe-finance.conf
server {
    listen 80;
    server_name faucet.pepesafe.finance;
    location / {
        proxy_pass http://127.0.0.1:8000;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
        proxy_read_timeout 36000;

        # WebSocket support
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection "upgrade";
    }
}
systemctl enable nginx
systemctl restart nginx

certbot --nginx -d faucet.pepesafe.finance