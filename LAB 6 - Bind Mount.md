## Bind Mount in Docker

### Task 1: Starting Docker Containers Bind Mounts
```
mkdir /home/ubuntu/share
```
```
echo 'Hello From Docker Host' > /home/ubuntu/share/index.html
```
```
docker run -it --name container1 -p 8080:80 -v /home/ubuntu/share:/var/www/html ubuntu:18.04 /bin/bash
```
```
apt-get update -y && apt-get install apache2 -y
```
```
service apache2 start
```
```
service apache2 status
```
```
curl localhost:80
```
```
echo 'Hello From Container1' > /var/www/html/index.html
```
```
curl localhost:80
```
Press Ctrl+P+Q, to switch back to Host
```
docker inspect container1
```
Check for keyword `Mounts`
```
docker run -it --name container2 -v /home/ubuntu/share:/var/www/html ubuntu:18.04 bash
```
```
echo 'Hello From Container2' > /var/www/html/index.html 
```
```
exit
```
```
docker ps -a
```
```
docker rm -vf container1 container2 
```
```
cat /home/ubuntu/share/index.html
```

### Task 2: Create a bind mount with --mount option and verify it
```
docker run -d --name container3 --mount type=bind,source=/home/ubuntu/share/,target=/app nginx:latest
```
```
docker inspect container3
```
```
docker exec -it container3 bash
```
```
cd app && ls
```
```
cat index.html
```
```
echo "hello from container3" > index.html
```
```
exit
```
```
cd /home/ubuntu/share/
```
```
cat index.html
```

