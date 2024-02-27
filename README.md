## Docker

```
docker run -it --rm --name node -v $(pwd):/app -w /app node:18-alpine npm run build
```

## Nginx

```
server {
  listen 80;
  server_name skullking.ir;
  server_name www.skullking.ir;
  return 301 https://$host$request_uri;
}

server {
  listen 443 ssl;
  ssl_certificate /etc/letsencrypt/live/skullking.ir/fullchain.pem;
  ssl_certificate_key /etc/letsencrypt/live/skullking.ir/privkey.pem;
  server_name skullking.ir;
  server_name www.skullking.ir;

  location / {
    proxy_pass http://127.0.0.1:3003/;
    proxy_set_header Host $host;
    proxy_set_header X-Forwarded-Host $http_host;
    proxy_set_header X-Real-IP $remote_addr;
    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
  }
}
```