# Networking

## Sockets Methods
* connect( (hostname, port) )
  + tuple of address and port
* recv(buffer)
* send(bytes)
* close() 

## TCP
* Transmission Control Protocol
* Reliable Connection based Protocol

tcpServer.py

```
import socket

def Main():
  host = '127.0.0.1'
  port = 5000
  
  s = socket.socket()
  s.bind((host, port))
  
  # Listen socket server at port 5000
  s.listen(1)
  c, address = s.accept()
  
  print("Connection from: " + str(addr))
  
  while True:
    data = c.recv(1024).decode('utf-8')
	if not data:  # client disconnect
	  break;
	  
    print("From connected user: " + data)
    data = data.upper()
    print("Sending: " + data)
    c.send(data.endcode('utf-8'))
  c.close()
  
# run 
if __name__ == '__main__':
  Main()  
```

tcpClient.py

```
import socket

def Main():
  host = '127.0.0.1'
  port = 5000
  
  s = socket.socket()
  s.connect((host, port))
  
  # get input from user
  message = input("->")
  
  while message != 'q':
    s.send(message.encode('utf-8'))     # send message
	data = s.recv(1024).decode('utf-8') # wait reply
	print("Received from server: " + data)
	
	# get new user data
	message = input("->")
	
  s.close()
  
if __name__ == '__Main__'
  Main()
	
```

run the script

`python3 tcpServer.py`
`python3 tcpCLient.py'

## UDP
* User Datagram Protocol
* Connectionless based protocol

udpServer.py

```
import socket

def Main():
  host = '127.0.0.1'
  port = 5000
  
  s = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)  # setup for UDP
  s.bind((host, port))   # binding the socket
  
  print("Server Started")
  
  while True:
    data, addr = s.recvfrom(1024)  # UDP receive, and client address
    data = data.decode('utf-8')
	
    print("Message From: " + str(addr)
	print("From connected user: " + data)
    data = data.upper()
    print("Sending: " + data)
    c.sendto(data.endcode('utf-8'), addr)  # UDP send to client address
  c.close()
  
# run 
if __name__ == '__main__':
  Main()  
```

udpClient.py

```
import socket

def Main():
  host = '127.0.0.1'
  port = 5001		# UDP need to configure a different port
  
  server = ('127.0.0.1', 5000)  # server address, port
  
  s = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)  # setup for UDP
  s.bind((host, port))  # binding to socket
  
  # get input from user
  message = input("->")
  
  while message != 'q':
    s.sendto(message.encode('utf-8'), server)     # send UDP message to server
	data = s.recvfrom(1024) # wait reply
	data = data.decode('utf-8') 
	print("Received from server: " + data)
	
	# get new user data
	message = input("->")
	
  s.close()
  
if __name__ == '__Main__'
  Main()
	
```

## note
* socket are blocking call. Thus need to use thread to make it Non-Blocking

