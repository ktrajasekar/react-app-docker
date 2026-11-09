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
