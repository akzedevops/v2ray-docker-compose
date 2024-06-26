# HTTP & SOCKS Protocols

The HTTP and SOCKS (SOCKS5) proxy protocols are appropriate for internal usage on the relay server and port forwarding.
They would be exposed to the `127.0.0.1` IP address without authentication.

## On Relay Server

The command below shows how to use the HTTP proxy on the relay server.

```shell
export {http,https}_proxy="http://127.0.0.1:2000"
export {HTTP,HTTPS}_PROXY="http://127.0.0.1:2000"

# This "curl" should return the upstream IP (not relay!)
curl ifconfig.io

# The "sudo" command needs the -E parameter to use HTTP/SOCKS proxy and other envs
sudo -E apt install docker

unset {http,https}_proxy
unset {HTTP,HTTPS}_PROXY
```

## On Local Devices

You can use the HTTP or SOCKS proxies on your local devices using port forwarding.
The following SSH command makes the HTTP proxy available to the local device and the private network it uses.

```shell
ssh -vNL 2000:0.0.0.0:2000 root@13.13.13.13
# ssh -vNL <LOCAL-HTTP-PORT>:<LOCAL-IP-ADDRESS>:<REMOTE-HTTP-PORT> <USER>@<BRIDGE-IP>

export {http,https}_proxy="http://127.0.0.1:2000"
export {HTTP,HTTPS}_PROXY="http://127.0.0.1:2000"
# ...
```

## In SSH Connections

You can add the following line to the `$HOME/.ssh/ssh_config` file to use the HTTP or SOCKS proxies in your SSH connections.

* HTTP: ```ProxyCommand /usr/bin/nc -X connect -x 127.0.0.1:HTTP_PROXY_PORT %h %p```
* SOCKS: ```ProxyCommand /usr/bin/nc -X 5 -x 127.0.0.1:SOCKS_PROXY_PORT %h %p```
