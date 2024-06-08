# cloudflare-docker-proxy

![deploy](https://github.com/ciiiii/cloudflare-docker-proxy/actions/workflows/deploy.yaml/badge.svg)

> If you're looking for proxy for helm, maybe you can try [cloudflare-helm-proxy](https://github.com/ciiiii/cloudflare-helm-proxy).

## Deploy

[![Deploy to Cloudflare Workers](https://deploy.workers.cloudflare.com/button)](https://deploy.workers.cloudflare.com/?url=https://github.com/ciiiii/cloudflare-docker-proxy)

1. fork this project
2. modify the link of the above button to your fork url
3. click the button, you will be redirected to the deploy page

## Config tutorial

1. use cloudflare worker host: only support proxy one registry
   ```javascript
   const routes = {
     "${workername}.${username}.workers.dev/": "https://registry-1.docker.io",
   };
   ```
2. use custom domain: support proxy multiple registries route by host. Have to host your domain DNS on cloudflare.

  ```bash
  # install install dependencies
  yarn
  # replace example.com to your domain in both src/index.js and wrangler.toml
  sed -i 's/example.com/${your_domain}/g' src/index.js
  sed -i 's/example.com/${your_domain}/g' wrangler.toml
  # deploy
  pnpx wrangler deploy
  ```

## Usage

```bash
docker pull docker.example.com/library/busybox:stable
# or
sudo mkdir -p /etc/docker
sudo tee /etc/docker/daemon.json <<-EOF
{
    "registry-mirrors": [
        "https://docker.example.com",
    ]
}
EOF
sudo systemctl daemon-reload
sudo systemctl restart docker
docker pull busybox:stable
```
