# Networking

## DHCP

In order to connect to the flight computer via a direct ethernet connection, your computer must be running a DHCP server on the newtork adapter that you are using.

### macOS

 - https://aleph0.com/computing/macosx/dhcp-setup/

 - https://medium.com/@tzhenghao/how-to-ssh-into-your-raspberry-pi-with-a-mac-and-ethernet-cable-636a197d055

### Linux

## Network Discovery

To find a flight computer with an unknown IP on the network, you must first know the IP of the network adapter. This can be found by running the command `ifconfig`.

### nmap

```
sudo nmap -n -sP <your network adapter>/24
```

### arp

```
arp -i <your network adapter> -a
```

## Connection

To properly connect to the flight computer, a series of ports must also be opened to allow network communication.

Local Port | Service | Flight Computer Port
---------- | ------- | --------------------
8080 | GUI | 80
8086 | InfluxDB | 8086
9090 | Rosbridge WebSocket | 9090

This can be done with the command:

```
ssh -L 8080:localhost:8080 -L 8086:localhost:8086 -L 9090:localhost:9090 user@<flight computer ip>
```