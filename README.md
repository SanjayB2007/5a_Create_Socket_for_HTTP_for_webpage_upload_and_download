# 5a_Create_Socket_for_HTTP_for_webpage_upload_and_download
## AIM :
To write a PYTHON program for socket for HTTP for web page upload and download
## Algorithm

1.Start the program.
<BR>
2.Get the frame size from the user
<BR>
3.To create the frame based on the user request.
<BR>
4.To send frames to server from the client side.
<BR>
5.If your frames reach the server it will send ACK signal to client otherwise it will send NACK signal to client.
<BR>
6.Stop the program
<BR>
## Program 
1. server.py
```
import socket

# Create socket
server_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)

# Bind to localhost and port
server_socket.bind(('127.0.0.1', 8080))

# Listen for connections
server_socket.listen(5)
print("Server is running on port 8080...")

while True:
    client_socket, addr = server_socket.accept()
    print("Connection from:", addr)

    request = client_socket.recv(1024).decode()
    print("Request:\n", request)

    # Handle GET request (Download)
    if request.startswith("GET"):
        try:
            with open("index.html", "r") as file:
                content = file.read()
            response = "HTTP/1.1 200 OK\n\n" + content
        except FileNotFoundError:
            response = "HTTP/1.1 404 Not Found\n\nFile not found"

        client_socket.sendall(response.encode())

    # Handle POST request (Upload)
    elif request.startswith("POST"):
        data = request.split("\r\n\r\n")[1]
        with open("uploaded.txt", "w") as file:
            file.write(data)

        response = "HTTP/1.1 200 OK\n\nFile uploaded successfully"
        client_socket.sendall(response.encode())

    client_socket.close()
```
2. client.py
```
import socket

client_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
client_socket.connect(('127.0.0.1', 8080))

# -------- DOWNLOAD (GET request) --------
get_request = "GET / HTTP/1.1\r\nHost: localhost\r\n\r\n"
client_socket.send(get_request.encode())

response = client_socket.recv(4096).decode()
print("Server Response (Download):\n", response)

client_socket.close()


# -------- UPLOAD (POST request) --------
client_socket = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
client_socket.connect(('127.0.0.1', 8080))

data = "This is uploaded data from client."

post_request = f"""POST / HTTP/1.1
Host: localhost
Content-Length: {len(data)}

{data}
"""

client_socket.send(post_request.encode())

response = client_socket.recv(4096).decode()
print("\nServer Response (Upload):\n", response)

client_socket.close()
```
3. index.html
```
<html>
<head><title>Test Page</title></head>
<body>
<h1>Hello from Server!</h1>
</body>
</html>
```
## OUTPUT
<img width="1041" height="273" alt="image" src="https://github.com/user-attachments/assets/770f24e2-5423-4973-ac2b-e2f251092b85" />

<img width="1052" height="236" alt="image" src="https://github.com/user-attachments/assets/d32bfc47-e17f-41a2-bc1f-8178305dac9e" />

<img width="1040" height="202" alt="image" src="https://github.com/user-attachments/assets/fe5f5476-6bcf-432d-b57a-62a586779cf7" />

## Result
Thus the socket for HTTP for web page upload and download created and Executed
