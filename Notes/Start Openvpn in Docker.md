# Command for start openvpn on docker

```bash
docker run -p 1194:1194/udp --cap-add=NET_ADMIN --device=/dev/net/tun --privileged  -it ubuntu:openvpn bash   
```

```bash
openvpn --log /etc/openvpn/client.log --config /etc/openvpn/client.conf --daemon
```
