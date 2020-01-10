# ss-libev-armhf
Shadowsocks-libev client with v2ray-plugin for RaspberryPi 3b
## Shadowsocks-libev Client Docker Image by pengshp

[shadowsocks-libev][1] is a lightweight secured socks5 proxy for embedded devices and low end boxes.

It is a port of [shadowsocks][2] created by @clowwindy maintained by @madeye and @linusyang.

Based on alpine with latest version [shadowsocks-libev](https://github.com/shadowsocks/shadowsocks-libev) and [simple-obfs](https://github.com/shadowsocks/simple-obfs) and [v2ray-plugin](https://github.com/shadowsocks/v2ray-plugin).

Docker images are built for quick deployment in various computing cloud providers.

For more information on docker and containerization technologies, refer to [official document][3].

## Prepare the host

If you need to install docker by yourself, follow the [official installation guide][4].

## Pull the image

```bash
$ docker pull pengshp/ss-libev-armhf
```

This pulls the latest release of shadowsocks-libev.

It can be found at [Docker Hub][5].

## Start a container

You **must create a configuration file**  `/etc/shadowsocks-libev/config.json` in host at first:

```
$ mkdir -p /etc/shadowsocks-libev
```

A sample in JSON like below:

```
{
    "server":"server_ip",
    "server_port":9000,
    "local_address": "0.0.0.0",
    "local_port":1080,
    "password":"xxxxxx",
    "timeout":300,
    "method":"aes-128-gcm",
    "fast_open":true,
    "nameserver":"1.0.0.1",
    "mode":"tcp_and_udp"
}
```

If you want to enable **simple-obfs**, a sample in JSON like below:

```
{
    "server":"server_ip",
    "server_port":9000,
    "local_address": "0.0.0.0",
    "local_port":1080,
    "password":"xxxxxxx",
    "timeout":300,
    "method":"aes-128-gcm",
    "fast_open":true,
    "nameserver":"1.0.0.1",
    "mode":"tcp_and_udp",
    "plugin":"obfs-server",
    "plugin_opts":"obfs=tls"
}
```

If you want to enable **v2ray-plugin**, a sample in JSON like below:

```
{
    "server":"server_ip",
    "server_port":9000,
    "local_address": "0.0.0.0",
    "local_port":1080,
    "password":"xxxxx",
    "timeout":300,
    "method":"aes-128-gcm",
    "fast_open":true,
    "nameserver":"1.0.0.1",
    "mode":"tcp_and_udp",
    "plugin":"v2ray-plugin",
    "plugin_opts":"server"
}
```

For more v2ray-plugin configrations please visit [v2ray-plugin usage][6].

This container with sample configuration `/etc/shadowsocks-libev/client.json`

There is an example to start a container that listens on `1080` :

```bash
$ docker run -d -p 1080:1080/tcp -p 1080:1080/udp --name ss-libev --restart=always -v /etc/shadowsocks-libev:/etc/shadowsocks-libev pengshp/ss-libev-armhf
```
### Use docker-compose
Get docker-compose.yml, then change SERVER_ADDR and PASSWORD.

Run these commands:
```bash
~$ firefox socks5://raspberrypi_ip:1080

# On arm client (192.168.1.254)
$ docker-compose up -d client-arm

# On any LAN PC (192.168.1.XXX)
$ curl -x socks5h://192.168.1.234:1080 https://www.youtube.com/
$ curl -x socks5h://192.168.1.254:1080 https://www.youtube.com/
```

**Warning**: The port number must be same as configuration and opened in firewall.

[1]: https://github.com/shadowsocks/shadowsocks-libev
[2]: https://shadowsocks.org/en/index.html
[3]: https://docs.docker.com/
[4]: https://docs.docker.com/install/
[5]: https://hub.docker.com/r/teddysun/shadowsocks-libev/
[6]: https://github.com/shadowsocks/v2ray-plugin#usage
