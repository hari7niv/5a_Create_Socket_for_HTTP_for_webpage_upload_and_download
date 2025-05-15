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
```
import socket

def send_request(host, port, request, is_binary=False):
    with socket.socket(socket.AF_INET, socket.SOCK_STREAM) as s:
        s.connect((host, port))
        s.sendall(request)
        response = b""
        while True:
            chunk = s.recv(4096)
            if not chunk:
                break
            response += chunk
    return response if is_binary else response.decode(errors="replace")

def upload_file(host, port, filename):
    with open(filename, 'rb') as file:
        file_data = file.read()

    content_length = len(file_data)
    headers = (
        f"POST /upload HTTP/1.1\r\n"
        f"Host: {host}\r\n"
        f"Content-Length: {content_length}\r\n"
        f"Content-Type: application/octet-stream\r\n"
        f"Connection: close\r\n\r\n"
    )
    request = headers.encode() + file_data
    response = send_request(host, port, request, is_binary=False)
    return response

def download_file(host, port, filename):
    request = (
        f"GET /{filename} HTTP/1.1\r\n"
        f"Host: {host}\r\n"
        f"Connection: close\r\n\r\n"
    ).encode()

    response = send_request(host, port, request, is_binary=True)

    try:
        header_end = response.index(b"\r\n\r\n")
    except ValueError:
        print("Invalid response received.")
        return

    body = response[header_end+4:]

    with open(filename, 'wb') as file:
        file.write(body)

    print("File downloaded successfully.")

if __name__ == "__main__":
    host = 'example.com'
    port = 80

    upload_response = upload_file(host, port, 'example.txt')
    print("Upload response:", upload_response)

    download_file(host, port, 'example.txt')
```
## OUTPUT
![image](https://github.com/user-attachments/assets/9ddc0835-9024-41fa-a8c8-c609bfeca1f1)

## Result
Thus the socket for HTTP for web page upload and download created and Executed
