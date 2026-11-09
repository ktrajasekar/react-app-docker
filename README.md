## Run Container

### Build the docker

`sh
sudo docker build -t react-app:latest .
`

### Run the docker

`sh
docker run -d --name react-app --restart unless-stopped -p 80:80 react-app:latest
`

### update later

`sh
cd my-react-app
git pull
docker build -t react-app:latest .
docker rm -f react-app
docker run -d --name react-app --restart unless-stopped -p 80:80 react-app:latest
docker image prune -f
`

### Docker Status

`sh
docker container ls
`

### Flow Diagram

```
  UBUNTU LAPTOP                GIT HOSTING                 UBUNTU or Cloud
 ┌────────────────┐         ┌───────────────┐      ┌──────────────────────────────────┐
 │  React source  │ git push│ GitHub/GitLab │ git  │  ~/react-app-docker (cloned repo)│
 │  + Dockerfile  │────────▶│     repo      │─pull▶│              │                   │
 │  + nginx.conf  │         └───────────────┘      │              ▼                   │
 └────────────────┘                                │   docker build -t react-app .    │
                                                   │   (Node builds → Nginx serves)   │
                                                   │              │                   │
                                                   │              ▼                   │
                                                   │   Container: Nginx  :80          │
                                                   │              ▲                   │
                                                   │              │ reverse proxy     │
                                                   │   Caddy  :443 (HTTPS)            │
                                                   └──────────────▲───────────────────┘
                                                                  │
                                                          Browser users
                                                   https://yourdomain.com
```

### Deploy flow

| #   | Where   | Command                                                                                         |
| --- | ------- | ----------------------------------------------------------------------------------------------- |
| 1   | Laptop  | `git add . && git commit -m "update" && git push`                                               |
| 2   | Droplet | `cd ~/react-app-docker && git pull`                                                             |
| 3   | Droplet | `docker build -t react-app:latest .`                                                            |
| 4   | Droplet | `docker rm -f react-app`                                                                        |
| 5   | Droplet | `docker run -d --name react-app --restart unless-stopped -p 127.0.0.1:3000:80 react-app:latest` |
| 6   | Droplet | `docker image prune -f`                                                                         |
