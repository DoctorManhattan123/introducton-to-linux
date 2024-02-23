# Network Fundamentals

## Network Interfaces

**Network interfaces** are what connect your computer to a network.
Network interfaces can correspond to _physical_ objects (like a network interface controller which is the hardware component that allows a computer to connect to a network via a cable or wireless connection).
They can also be virtual, i.e. only exist in software.

You can show all your network interfaces using the `ip link show` command.
On an average Linux machine, this might output something similar to:

```sh
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN mode DEFAULT group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
2: enp5s0: <NO-CARRIER,BROADCAST,MULTICAST,UP> mtu 1500 qdisc fq_codel state DOWN mode DEFAULT group default qlen 1000
    link/ether 12:34:56:78:9a:bc brd ff:ff:ff:ff:ff:ff
3: wlp4s0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue state UP mode DORMANT group default qlen 1000
    link/ether 21:32:43:ab:cd:ef brd ff:ff:ff:ff:ff:ff
```

Here `enp5s0` is the interface that will be used for Ethernet connections, while `wlp4s0` is the interface that will be used for wireless connections.
Note that `enp5s0` is `DOWN`, while `wlp4s0` is `UP` indicating that the machine is connected to a wireless network and not Ethernet.

The `lo` interface is an example of a virtual interface called the **loopback device**.
It's useful if you need services that are running on the same machine to talk to each other.

## IP

The **Internet Protocol** (IP for short) is the basic protocol that allows the delivery of packets from one computer to another.
This protocol itself is unreliable, i.e. packets might get corrupted, lost or duplicated.
You will therefore usually use higher-level protocols like TCP to ensure that this doesn't happen.
However because they build on top of IP, it is still useful to roughly understand what IP does.

The Internet Protocol works by utilizing IP addresses.
Each network interface is assigned a unique **IP address** which consists of 4 numbers from 0 to 255 separated by dots.
For example a network interface `iface` might have the IP address `123.57.81.109`.
If you send an IP packet to `123.57.81.109`, it would arrive at the network interface `iface`.

You can find out the IP address of a network interface by using the `ip addr show` command.
For example to find out the IP address of `wlp4s0`, you could do:

```sh
ip addr show wlp4s0
```

This will output something similar to:

```
3: wlp4s0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue state UP group default qlen 1000
    link/ether 21:32:43:ab:cd:ef brd ff:ff:ff:ff:ff:ff
    inet 10.37.1.116/24 brd 10.37.1.255 scope global dynamic noprefixroute wlp4s0
       valid_lft 84671sec preferred_lft 84671sec
```

From this we can infer that on the given machine, the wireless interface `wlp4s0` has the IP address `10.37.1.116`.

There are a few IP addresses with special meaning.

For example the IP address `127.0.0.1` can be used to address the current machine.

A few IP addresses are **private addresses**, i.e. they can only be used within local networks (and will not be routed on the public internet).
These are:

- `10.0.0.0` - `10.255.255.255`
- `172.16.0.0` - `172.31.255.255`
- `192.168.0.0` - `192.168.255.255`

If you look at the IP address from above, you will realize that this is one of the private addresses.
In fact that will usually be the case, if you are on a regular machine connected to some router.
If you have a router, the machines connected to it will be in some local network and have private IP addresses only.

This begs the question - if your machine has a private IP address and private IP addresses are not routed on the public internet, then why can you browse sites that are part of the public internet?

The answer lies within dark magic called **Network Address Translation** (NAT for short).
A normal router has at least two interface - a "private" interface for communicating with machines in the local network and a "public" interface for communicating with machines on the public internet.
The "private" interface will have a private IP address and be on the same network as the machines connected to the router.
The "public" interface will have a public IP address.

Whenever some machine in the local network tries to send a packet to a machine on the public internet, the router will replace the private IP of the machine with the public IP of the router.
When the IP packet arrives at the destination, it will see the IP of the router and send a response to the router.
The router will then forward that IP packet to the correct machine on the local network.

You can observe this in practice.
Go to any "What is my IP?" website, such as whatismyip.com.
You will see the public IP address of your router, _not_ the private IP address of your network interface.

Note that because private IP addresses can only be used in local networks and local networks are isolated from each other, different devices in different networks might have the same private IP address.

> Note that we only talked about IPv4.
> This protocol is currently slowly being superseded by IPv6, however this is out of scope for this book.

## TCP and UDP

The **Transmission Control Protocol** (TCP for short) is a higher-level protocol that builds on top of IP.
Unlike IP, TCP is reliable and ensures that packets arrive in the correct order.
This makes it ideal for most scenarios where you need to ensure reliability.
For example most websites use TCP under the hood - after all, you don't want to get scrambled or missing content when visiting a website.

TCP is also connection-oriented.
This means that a connection must be established between client and server before sending any packets.
Additionally, connections will be explicitly terminated.

The **User Datagram Protocol** (UDP) is another protocol that builds on top of IP.
Unlike TCP it does not provide reliablity and is commonly used for applicating like video streaming.

TCP and UDP use **sockets**, which are endpoints for sending and receiving data.
Sockets are associated with **ports** - a port is just a number that can be used to uniquely identify a connection endpoint.
For example HTTPS servers for serving websites typically run on port 443.

Both TCP and UDP function based on a client-server setup.

On the server, we construct a socket that binds to a port, listens for incoming connections and accepts them.
On a client, we initiate the connection to a server (using its IP address and port number).

Once a connection is established, data can be sent between the client and the server.
Finally, the connection is closed.

Here is an example for a server socket that uses TCP:

```python
import socket

server_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
server_socket.bind(('127.0.0.1', 12345))  # Bind to 127.0.0.1 and port 12345
server_socket.listen(1)  # Listen for 1 connection
print("Server is waiting for connections...")

conn, addr = server_socket.accept()  # Accept a connection
print(f"Connected by {addr}")

while True:
    data = conn.recv(1024)  # Receive data from client
    if not data:
        break
    conn.sendall(data)  # Echo back the received data

conn.close()
```

Here is an example for a client socket that uses TCP:

```python
import socket

client_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
client_socket.connect(('127.0.0.1', 12345))  # Connect to the server

message = "Hello, server!"
client_socket.sendall(message.encode())  # Send a message

response = client_socket.recv(1024)  # Receive a response
print(f"Received: {response.decode()}")

client_socket.close()
```

## HTTP

HTTP builds on top of TCP.
HTTP is a request-response protocol, i.e. HTTP clients send requests to an HTTP server and receive a response in return.

HTTP knows many different request methods, the two most common of which are **GET** and **POST**.
GET requests are generally used to retrieve data.
POST requests are generally used to send information to the server that tell it to update some information.
