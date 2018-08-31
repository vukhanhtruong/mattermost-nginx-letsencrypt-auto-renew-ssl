## Getting Started

#### First, create a Docker network. This enables container DNS, which allows containers to communicate with one another by name.

```
docker network create mattermostnw
```

#### Customize the nginx-proxy settings
- Read the official Mattermost docker [here](https://github.com/mattermost/mattermost-docker)
- Read the Nginx-proxy documentation [here](https://github.com/jwilder/nginx-proxy#replacing-default-proxy-settings)


#### Start the Nginx proxy

```
docker run --name nginx-proxy --net mattermostnw -p 80:80 -p 443:443 -v /path/to/custom.conf:/etc/nginx/conf.d/my_proxy.conf:ro -v ~/certs:/etc/nginx/certs -v /etc/nginx/vhost.d -v /usr/share/nginx/html -v /var/run/docker.sock:/tmp/docker.sock:ro --label com.github.jrcs.letsencrypt_nginx_proxy_companion.nginx_proxy -d --restart always jwilder/nginx-proxy
```

**Note: Remember change the mounted paths to your own**


#### Start the Let’s Encrypt Nginx Proxy Companion.

```
docker run --name letsencrypt-nginx-proxy-companion --net mattermostnw -v ~/certs:/etc/nginx/certs:rw -v /var/run/docker.sock:/var/run/docker.sock:ro --volumes-from nginx-proxy -d --restart always jrcs/letsencrypt-nginx-proxy-companion
```

### Start Mattermost
- Pulling the latest Mattermost docker
- Copy override `docker-compose.yml` to mattermost-docker folder
- Run `docker-compose build && docker-compose up -d`

## Credits
- [mattermost-docker](https://github.com/mattermost/mattermost-docker)
- [nginx-proxy](https://github.com/jwilder/nginx-proxy)
- [docker-letsencrypt-nginx-proxy-companion](https://github.com/JrCs/docker-letsencrypt-nginx-proxy-companion)
