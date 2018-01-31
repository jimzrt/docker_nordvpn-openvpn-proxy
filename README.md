[![logo](https://nordvpn.com/wp-content/themes/nordvpn-bootstrap/build/1517241007/images/logo.png)](https://nordvpn.com)

# Privoxy via NordVPN
An Alpine Linux container running Privoxy and OpenVPN via NordVPN

Protect your browsing activities through an encrypted and anonymized VPN proxy!

You will need a [NordVPN](https://nordvpn.com) account. 

## Starting the VPN Proxy

```Shell
docker run -d \
--cap-add=NET_ADMIN \
--device=/dev/net/tun \
--name=vpn_proxy \
--dns=209.222.18.218 --dns=209.222.18.222 \
--restart=always \
-e "REGION=<region>" \
-e "USERNAME=<nordvpn_username>" \
-e "PASSWORD=<nordvpn_password>" \
-e "LOCAL_NETWORK=192.168.1.0/24" \
-v /etc/localtime:/etc/localtime:ro \
-p 8118:8118 \
jamestop/nordvpn-openvpn-proxy 
```

Substitute the environment variables for `REGION`, `USERNAME`, `PASSWORD`, `LOCAL_NETWORK` as indicated.

A `docker-compose-dist.yml` file has also been provided. Copy this file to `docker-compose.yml` and substitute the environment variables are indicated.

Then start the VPN Proxy via:

```Shell
docker-compose up -d
```

### Environment Variables
`REGION` is optional. The default region is set to `us1994`. `REGION` should match the supported NordVPN `.opvn` region config. 

`USERNAME` / `PASSWORD` - Credentials to connect to NordVPN

`LOCAL_NETWORK` - The CIDR mask of the local IP addresses (e.g. 192.168.1.0/24, 10.1.1.0/24) which will be acessing the proxy. This is so the response to a request can be returned to the client (i.e. your browser).

## Connecting to the VPN Proxy

To connect to the VPN Proxy, set your browser proxy to 0.0.0.0:8118.

If you're using Chrome, you may want to use [ProxySwitchyOmega](https://chrome.google.com/webstore/detail/proxy-switchyomega/padekgcemlokbadohgkifijomclgjgif)

If you're using Firefox, you may want to use [ProxySwitcher](https://addons.mozilla.org/en-US/firefox/addon/proxy-switcher/)