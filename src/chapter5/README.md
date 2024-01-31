# Network Fundamentals

## Network Interfaces

**Network interfaces** are what connect your computer to a network.
Network interfaces can correspond to _physical_ objects (like a network interface controller).
They can also be virtual, i.e. only exist in software.

You can show all your network interfaces using the `ip a` command.

## IP

The **Internet Protocol** (IP for short) is the basic protocol that allows the delivery of packets from one computer to another.
Each network interface is assigned a unique **IP address** which consists of 4 numbers from 0 to 255 separated by dots.
For example a network interface might have the IP address `123.57.81.109`.

A special IP address is `127.0.0.1`.

If you look at the output of `ip a`, you will see the IP address of each network interface.

Note that IP protocol itself is unreliable, i.e. packets might get corrupted, lost or duplicated.

## TCP and UDP

The **Transmission Control Protocol** (TCP for short) is a that builds on top of IP.
Unlike IP, TCP is reliable and ensures that packets arrive in the correct order.

TCP is also connection-oriented.
This means that a connection must be established between client and server before sending any packets.

This is achieved with **sockets**, which are endpoints for sending and receiving data.
On the server, we construct a socket that binds to a **port**, listens for incoming connections and accepts them.
On the client, we initiate the connection to a server (using its IP address and port number).

Once a connection is established, data can be send between the client and the server.
Finally, the connection is closed.

Here is an example for a server socket:

```python
import socket

server_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
server_socket.bind(('localhost', 12345))  # Bind to localhost and port 12345
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

Here is an example for a client socket:

```python
import socket

client_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
client_socket.connect(('localhost', 12345))  # Connect to the server

message = "Hello, server!"
client_socket.sendall(message.encode())  # Send a message

response = client_socket.recv(1024)  # Receive a response
print(f"Received: {response.decode()}")

client_socket.close()
```

The **User Datagram Protocol** (UDP) ensures is another protocol that builds on top of IP.
Unlike, TCP it does not provide reliablity.

## HTTP

HTTP builds on top of TCP.
HTTP is a request-response protocol, i.e. HTTP clients send requests to an HTTP server and receive a response in return.

HTTP knows many different request methods, the two most common of which are **GET** and **POST**.
GET requests are generally used to retrieve data.
POST requests are generally used to send information to the server that tell it to update some information.
