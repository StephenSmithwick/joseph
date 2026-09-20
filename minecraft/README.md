# colima
```zsh
colima start --port-forwarder grpc
colima ls
```

# minecraft container
```zsh
docker stop mc-server && docker rm mc-server 
docker run -d -it --name mc-server \
-e EULA=TRUE \
-p 19132:19132/udp \
-p 19132:19132 \
-p 19133:19133/udp \
-v mc-volume:/data \
itzg/minecraft-bedrock-server
```
- `docker ps -a` - list all containers even stopped ones
- `docker start mc-server` - restart mc-server

look at the logs
```zsh
docker logs -f mc-server
```

# Verifying server
```zsh
pipx run mcstatus --bedrock localhost:19132 status
```


# networking
- `ipconfig getiflist` list interfaces
- `networksetup -listallhardwareports` more information about our interfaces
- `ifconfig bridge0` Inspect the bridge
- `route get default` Which interface is being used
- `ipconfig getifaddr "$(route get default | awk '/interface:/{print $2}')"` All in one get ip address of deault route
- `docker inspect -f '{{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}' mc-server` get container's ip address
-  `docker run --rm --network container:mc-server nicolaka/netshoot \
  tcpdump -ni any udp port 19132` - tcpdump in a container within same network namespace
- `sudo tcpdump -ni lo0 udp port 19132` - tcpdump on laptop
- `ipconfig getifaddr en0` machine ip addrss

# Minecraft Server Console
- `docker attach mc-server` connect
- `allowlist add TopJewel5484` Add Daddy
- `allowlist add PutTitan6514` Add Caleb
- `allowlist list` list users on allowlist]
- `Ctrl + P followed immediately by Ctrl + Q` disconnect

## Turn off allowlist
```zsh
docker exec mc-server sh -c 'sed -i "s/^allow-list=.*/allow-list=false/" /data/server.properties'
docker restart mc-server
```
