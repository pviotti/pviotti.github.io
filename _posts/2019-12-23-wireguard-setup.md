---
layout: post
title: "Setting up a VPN between Ubuntu and Arch with WireGuard"
date: 2019-12-23 20:00
categories: hacks
---

[WireGuard][wireguard] is a modern VPN software that uses state-of-the-art cryptographic
algorithms and is designed to be significantly simpler to setup and faster than
its main competitor, OpenVPN.
In this post I'm going to give a rundown of my configuration of WireGuard for
securing a VPN tunnel between a "client" running **Arch Linux** and and a "server" running
**Ubuntu Server 19.10**.[^1]

Most of this post is a summary and adaptation of the following documents,
which are very helpful and worth reading:
 - [Linode guide](https://www.linode.com/docs/networking/vpn/set-up-wireguard-vpn-on-ubuntu/)
 - [Arch Linux WireGuard page](https://wiki.archlinux.org/index.php/WireGuard)
 - [WireGuard docs on GitHub](https://github.com/pirate/wireguard-docs)

# Ubuntu 19.10 (server) setup

While on the older Ubuntu releases it was necessary to install a PPA repository,
Ubuntu 19.10 includes WireGuard in its official *universe* repository. So to install
it, it is sufficient to issue:

    sudo apt install wireguard

followed by:

    sudo modprobe wireguard

to load its kernel module. To verify that it's been loaded, check that the
following command doesn't return an empty result:

    lsmod | grep wireguard

Next we generate the private and public key of the server with:

    wg genkey | tee privatekey | wg pubkey > publickey

Make sure to restrict access to the private key:

    chmod 600 privatekey

We decide a port to use for the server, which in the following example will be
marked as `$server_port`, and enable its use on the local firewall:

    sudo ufw allow $server_port/udp

You can check the status of the firewall by issuing:

    sudo ufw status

The configuration of WireGuard is all kept in `/etc/wireguard/wg0.conf`, which
for the server, will be similar to the following:

    [Interface]
    Address = $server_private_ip/24
    PrivateKey = $server_private_key
    PostUp = iptables -A FORWARD -i wg0 -j ACCEPT; iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE; ip6tables -A FORWARD -i wg0 -j ACCEPT; ip6tables -t nat -A POSTROUTING -o eth0 -j MASQUERADE
    PostDown = iptables -D FORWARD -i wg0 -j ACCEPT; iptables -t nat -D POSTROUTING -o eth0 -j MASQUERADE; ip6tables -D FORWARD -i wg0 -j ACCEPT; ip6tables -t nat -D POSTROUTING -o eth0 -j MASQUERADE
    ListenPort = $server_port

    [Peer]
    PublicKey = $client_public_key
    AllowedIPs = $client_private_ip/32

where:
 - `$client_private_ip` is an arbitrarily chosen private IP address of the client
 - `$client_public_key` is the public key of the client (see below)
 - `$server_private_ip` is an arbitrarily chosen private IP address of the
   server, in the same subnet of the private IP address of the client
 - `$server_private_key` is the private key of the server, as created in the
   step above

To enabled the forwarding of IPv4 and IPv6 traffic issue:

    sudo sysctl -w net.ipv6.conf.all.forwarding=1
    sudo sysctl -w net.ipv4.ip_forward=1

and change the corresponding lines in `/etc/sysctl.d/99-sysctl.conf` to make
this configuration durable.

Finally, let's start WireGuard:

    wg-quick up wg0

This command will make systemd start the WireGuard service at boot:

    sudo systemctl enable wg-quick@wg0

# Arch Linux (client) setup

On the Arch Linux client, to install WireGuard issue:

    sudo pacman -S wireguard

Then, proceed to create public and private key exactly as in the server case
(see above).

The client configuration stored in `/etc/wireguard/wg0.conf` will be similar to:

    [Interface]
    Address = $client_private_ip/24
    PrivateKey = $client_private_key

    [Peer]
    PublicKey = $server_public_key
    AllowedIPs = 0.0.0.0/0, ::/0
    Endpoint = $server_public_ip:$server_port

where
 - `$client_private_key` is the private key of the client we just generated
 - `$server_public_key` is the public key of the server (see above)
 - `$server_public_ip` is the public IP address of the server
 - `0.0.0.0/0, ::/0` means that all traffic will be forwarded through the VPN tunnel

Finally, let's start WireGuard on the client too:

    wg-quick up wg0

# Connectivity check

To check that the tunnel is actually established we can use the `wg` command:

    sudo wg show

on both client and server.
A quick check of the exposed IP address, for instance with `curl ifconfig.me`,
will confirm that our network traffic is now passing through the server.

# Cost analysis

A back-of-the-envelope calculation reveals that the price of this setup
placing the VPN server on a virtual machine in a cloud computing platform is
comparable to (if not less than) the price of a commercial VPN service.
The VPN server, being mostly computation and networking-heavy, won't need much
memory or storage. In my experience, even a single core is more than enough for
individual daily use. The [ballpark estimate][azure-price-calculator]
of the price for such setup is around 10€/month, which is considerably further
reduced by leveraging offers such as [spot instances][azure-spot-vms] and
[free tiers][azure-free].

[^1]: for WireGuard and its official documentation, both client and server are
    simply "peers".

 [wireguard]: https://www.wireguard.com/
 [azure-price-calculator]: https://azure.microsoft.com/en-us/pricing/calculator/
 [azure-spot-vms]: https://docs.microsoft.com/en-us/azure/virtual-machines/windows/spot-vms
 [azure-free]: https://azure.microsoft.com/en-us/free/

