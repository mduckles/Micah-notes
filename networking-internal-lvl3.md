---
id: "networking-internal-lvl3"
aliases:
  - "Networking-Internal  "
tags: []
---

### Networking-Internal  

## Creating File Server
after doing some research about creating a file server I found two good options.
First to create a [[FTP]](File Transfer Protocol) server on the windows machine while this is probably the easiest option because set up on a windows machine with settings it does come with some down sides most of these are security related as data transferred is not encrypted and makes hacking through all machines connected to  the server a lot easier.
The second option is a nas like truenas which solves the a lot of the security problems and the nas will be it's own server so processing power can be replaced with extra storage meaning more storage for the same cost this also allows for use of things like raidZ3 to keep all data safe even with multiple hard drive failures which is good for the user truenas also offers a web interface once set up which is very useful for setting up user permission and drives .

So seeing this the this the obvious choice for the end user is to use a nas like truenas but at first I write this off as all the equipment I was given was a windows machine and thought if I had the hardware I would use a nas but then I resized I could use a virtual machine to simulate using truenas which worked quite well   



## [[OSI]] Model  
### Physical Layer (Layer 1):
At the Physical Layer, the network uses Ethernet (IEEE 802.3) as the physical medium or the virtualised equivalent, typically with twisted-pair copper cables or research optic cables. The devices, such as Windows Machine(1)  , the TrueNAS server, and Windows Machine(2) , are physically connected to the switch using Ethernet cables. this layer is the simplest.

### Data Link Layer (Layer 2):
The Data Link Layer is where devices are identified by their unique Media Access Control ([[MAC]]) addresses. When Windows Machine(1) communicates with the TrueNAS server or Windows Machine(2), it uses MAC addresses to ensure data is sent to the correct destination this does have its flaws. The switch maintains a MAC address table that associates MAC addresses with the corresponding switch ports. This allows the switch to forward data frames directly to the intended recipient.

### Network Layer (Layer 3):
At the Network Layer, the network uses the Internet Protocol version 4 (IPv4) or (IPv6 works pretty much the same but allows for more adresses) for addressing and routing. Each device in the network has an IPv4 and IPv6 address, allowing them to communicate with each other. For instance, the TrueNAS server has the IP address 10.160.135.22, Windows Machine(1) could have 10.160.135.TODO, and Windows Machine(2) could have 10.160.135.TODO.

When Windows Machine(1) wants to access files on the TrueNAS server, it first needs to know the MAC address associated with the IP address of the TrueNAS server. It does this by sending an Address Resolution Protocol (ARP) request to the local network. The ARP request asks, "Who has IP address 10.160.135.22?" The TrueNAS server replies with its MAC address, and Windows Machine(1) stores this information in its ARP cache. 

### Transport Layer (Layer 4):
At the Transport Layer, the network primarily uses the Transmission Control Protocol (TCP) for reliable data transfer. When Windows Machine(1) wants to share a file with the TrueNAS server, it establishes a TCP connection with the server to ensure data integrity.

For example, let's say Windows Machine(1) uses the File Explorer to copy a file to the TrueNAS server. The following steps occur:
    Windows Machine(1) sends a TCP SYN packet to the TrueNAS server to initiate a connection.
    The TrueNAS server responds witha TCP SYN-ACK packet, acknowledging the request.
    Windows Machine(1) sends an ACK packet to complete the three-way handshake, and the connection is established.
    Windows Machine(1) starts sending the file data to the TrueNAS server in TCP segments.
    The TrueNAS server acknowledges the receipt of each segment, and if any segment is lost or corrupted during transmission, Windows Machine(1) retransmits it.
    Once the file transfer is complete, the connection is closed with a TCP FIN packet.

### Session Layer (Layer 5):
The Session Layer is responsible for establishing, maintaining, and terminating sessions between applications on different devices. It ensures that data exchange between applications is properly synchronized. So that each packet isn't blindly sent.

In the file-sharing network, the Session Layer might not be heavily utilized for simple file sharing between the two Windows machines and the TrueNAS server. The file sharing process can be viewed as a "session" in the context of the OSI model, where the session begins when Windows Machine(1) establishes a connection to the TrueNAS server and ends when the file transfer is completed and the connection is closed. this is how the whole file gets transferd in little packets while staying together.


### Presentation Layer (Layer 6):
The Presentation Layer handles data formatting, compression, and encryption to ensure that data from the application layer of one device can be understood by the application layer of another device. this is important for security

In the context of file sharing, the Presentation Layer might be involved in data conversion or encryption if specific requirements exist. For example:
    Data Formatting: If Windows Machine(1) sends a file in a specific format (e.g., ASCII text), the Presentation Layer may need to convert it to a different format (e.g., Unicode) for the TrueNAS server to understand it correctly.
    Data Compression: The Presentation Layer can compress the data before transmission to reduce the file size and optimize bandwidth usage.
    Data Encryption: If security is a concern, the Presentation Layer can encrypt the file data to ensure confidentiality during transmission.

Some protocols like Secure Sockets Layer (SSL) or Transport Layer Security (TLS) operate at the Presentation Layer to provide encryption and secure data exchange. However, their usage depends on the specific configuration and security requirements of the network.

Application Layer (Layer 7):
At the Application Layer, the network uses the following protocols for file sharing:

Server Message Block (SMB) / Common Internet File System (CIFS):
SMB is a network file-sharing protocol primarily used by Windows devices to access files, printers, and other resources on remote servers. When Windows Machine(1) wants to access shared files on the TrueNAS server, it establishes a session with the server using SMB. SMB allows Windows Machine(1) to send commands and requests to the TrueNAS server to read, write, and manage files. The TrueNAS server responds to these requests and grants access to the shared resources based on user credentials.

Credentials: Windows Machine(1) provides its Windows username and password as credentials when connecting to the TrueNAS server. The TrueNAS server validates these credentials to determine whether the user has the necessary permissions to access the shared files.

Network File System (NFS):
NFS is a file-sharing protocol commonly used in Unix-like operating systems. Windows Machine(1) can access files on the TrueNAS server using NFS if NFS is enabled and configured on the TrueNAS server. NFS allows Windows Machine(1) to mount the shared directories from the TrueNAS server as if they were local directories on Windows Machine 1.

Credentials: To access the NFS shares, Windows Machine(1) needs to provide appropriate NFS credentials, which may include a username and password or a secure NFS client configuration.








