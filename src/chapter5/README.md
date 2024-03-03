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

### HTTP Basics

HTTP builds on top of TCP.

HTTP is a **server-client protocol**.
A client (for example a web browser) communicates with a server (for example some Python service).

HTTP is a request-response protocol, i.e. HTTP clients send requests to an HTTP server and receive a response in return.
Requests are pretty much always sent by the HTTP client.

HTTP knows many different request methods, the two most common of which are **GET** and **POST**.
GET requests are generally used to retrieve data.
POST requests are generally used to send information to the server that tell it to update some information.

For example, let's use `curl` to retrieve `https://example.com`:

```sh
curl --trace-ascii example.txt http://example.com
```

Looking at `example.txt`, we will see:

```
GET / HTTP/1.1
Host: example.com
User-Agent: curl/7.81.0
Accept: */*
```

The request contains:

The HTTP method that defines the operation the client wants to perform.

The path of the resource to fetch (`/` in this case).
Note that the path is "relative" to the origin.

The version of the HTTP protocol (`HTTP/1.1` in this case).

This is followed by optional headers.
For example in this case we inform the server that our User-Agent is `curl/7.81.0`.

The response:

```
HTTP/1.1 200 OK
Accept-Ranges: bytes
Age: 343881
Cache-Control: max-age=604800
Content-Type: text/html; charset=UTF-8
Date: Sun, 03 Mar 2024 09:05:25 GMT
Etag: "3147526947"
Expires: Sun, 10 Mar 2024 09:05:25 GMT
Last-Modified: Thu, 17 Oct 2019 07:18:26 GMT
Server: ECS (nyd/D146)
Vary: Accept-Encoding
X-Cache: HIT
Content-Length: 1256

<!doctype html>
...
</html>
```

This contains the protocol version, the status code, the status text.
This is followed by the headers.
This is followed by the data.

Status codes

### Headers

The most important request headers are the following.

The `Host` header specifies the host (and optionally port) of the server to which the request is being sent.
This header is mandatory.

The `User-Agent` header is a string that tells servers information about the client.
This could include the application, operating system, vendor and version of the client.
For example:

```
Mozilla/5.0 (X11; Ubuntu; Linux x86_64; rv:123.0) Gecko/20100101 Firefox/123.0
```

The `Accept` header describes which content types (MIME types) a client can understand:

```
text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,*/*;q=0.8
```

The `Accept-Language` header specifies the natural language that the client prefers:

```
en-US,en;q=0.5
```

The `Content-Type` header indicates the type of the request body (usually used with `POST` requests).

The most important response headers are the following.

The `Content-Type` header specifies the media type of the response content.
For example if the server sends back an HTML, this would be the `Content-Type`:

```
text/html; charset=UTF-8
```

The `Content-Length` indicates the size of the message body in bytes.

### Cookies

Cookies are data the a server can send to a web browser.
The web browser can store the cookie and send it back to the server with later requests.

Cookies are useful for remembering information between requests.
There are three primary functions:

- session management
- personalization
- tracking

Note that cookies should not be used for general storage (local storage and session storage should be used instead).

Here is the Flask app:

```python
from flask import Flask, request, make_response, render_template_string

app = Flask(__name__)

html_content = """
<!DOCTYPE html>
<html>
<head>
    <title>Cookie Test</title>
</head>
<body>
    <button id="setCookieBtn">Set Cookie</button>
    <button id="getCookieBtn">Get Cookie</button>
    <script>
        document.getElementById('setCookieBtn').onclick = function() {
            fetch('/set-cookie')
                .then(response => response.text())
                .then(data => console.log(data));
        };

        document.getElementById('getCookieBtn').onclick = function() {
            fetch('/get-cookie')
                .then(response => response.text())
                .then(data => console.log(data));
        };
    </script>
</body>
</html>
"""

@app.route('/')
def home():
    return render_template_string(html_content)

@app.route('/set-cookie')
def set_cookie():
    resp = make_response("Cookie is set")
    resp.set_cookie('example_cookie', 'Example')
    return resp

@app.route('/get-cookie')
def get_cookie():
    cookie_value = request.cookies.get('example_cookie', 'Cookie not found')
    return f'Cookie value: {cookie_value}'

if __name__ == '__main__':
    app.run(debug=True)
```

If you click the `Set Cookie` button, you will see the following response header:

```
Set-Cookie: example_cookie=Example; Path=/
```

If you click the `Get Cookie` button, you will see the following request header:

```
example_cookie=Example
```

### Redirects

HTTP Redirects allow you to redirect a client to another page.

Redirects have a `3xx` status code and a `Location` response header indicating the URL to redirect to.
