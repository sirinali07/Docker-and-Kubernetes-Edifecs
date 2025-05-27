## Docker Networking
### Task 1: Create containers and check the connectivity between them.
```
docker network ls
```
```
docker run -d --name ct1 alpine sleep 3600
```
```
docker run -d --name ct2 alpine sleep 3600
```
```
docker ps
```
```
docker inspect network bridge
```
```
docker exec -it ct1 sh
```
```
ping <IP address of ct2> -c 5
```
```
ping ct2
```
Press Ctrl+P+Q, to switch back to Host
```
docker exec -it ct2 sh
```
```
ping <IP address of ct1> -c 5
```
```
ping ct1
```
Press Ctrl+P+Q, to switch back to Host


### Task 2: Create a new docker bridge and check connectivity between containers of same bridge
```
docker network create ct-bridge1    # docker network create --driver bridge ct-bridge1
```
```
docker network inspect ct-bridge1
```
```
docker network ls
```
```
docker run -it --network ct-bridge1 --name=ct3 busybox
```

Press Ctrl+P+Q, to switch back to Host
```
docker run -it --network ct-bridge1 --name=ct4 busybox
```
Press Ctrl+P+Q, to switch back to Host
```
docker network inspect ct-bridge1
```
```
docker ps
```
```
docker attach ct3
```
```
ip addr
```
```
ping -c 5 ct4
```
```
ping <IP address of ct1> -c 5
```
Press Ctrl+P+Q, to switch back to Host
```
docker exec -it ct1 sh
```
```
ping <ip addr of ct3> -c 5
```
Press Ctrl+P+Q, to switch back to Host


### Task 3: Create a new docker bridge and check connectivity between containers of different bridges
```
docker network create --driver bridge ct-bridge2
```
```
docker run -it --network ct-bridge2 --name=ct5 busybox
```
Press Ctrl+P+Q, to switch back to Host
```
docker run -it --network ct-bridge2 --name=ct6 busybox
```
Press Ctrl+P+Q, to switch back to Host
```
docker attach ct5
```
```
ping -c 5 ct6
```
```
ip addr
```
```
ping -c 5 ct3
```
```
ping -c 5 ct4
```

Press Ctrl+P+Q, to switch back to Host


### Task 4: Using 'Docker network connect' command create a successful connection between containers of different bridges
```
docker network ls
```
```
docker network connect ct-bridge1 ct1
```
```
docker network inspect ct-bridge1
```
```
docker exec -it ct1 sh
```
```
ping -c 5 ct5
```
Press Ctrl+P+Q, to switch back to Host
```
docker network connect ct-bridge1 ct5
```
```
docker network inspect ct-bridge1
```


### Task 5: Launch a container to host network
```
docker run -d --network host --name=ct7 nginx
```
```
docker ps -a
```
```
docker exec -it ct7 sh
```
Also to check the network connection to the outside world we can run the below commands
```
apt update
```
Download the ping utility if not already available
```
apt install inetutils-ping
```
```
ping 8.8.8.8
```
Download the curl utility if not already available
```
apt install curl
```
```
curl https://8.8.8.8
```
Also if you check the default port 80, the container would be accessible although we have not published the container

Press Ctrl+P+Q, to switch back to Host
```
docker network inspect host
```
```
docker run -d --network host --name=ct8 httpd
```
```
docker ps -a
```


### Task 6: Launch a container to none network 
```
docker run -it --network none --name=ct11 centos bash
```
```
ip addr
```
```
ping 8.8.8.8
```
Press Ctrl+P+Q, to switch back to Host
```
docker network inspect none
```

