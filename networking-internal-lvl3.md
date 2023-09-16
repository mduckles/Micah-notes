---

id: "networking-internal-lvl3"
aliases:
  - "Networking-Internal  "
tags: []
---


## Creating File Server
after doing some research about creating a file server I found two good options.
First to create a [[FTP]](File Transfer Protocol) server on the windows machine while this is probably the easiest option because set up on a windows machine with settings it does come with some down sides most of these are security related as data transferred is not encrypted and makes hacking through all machines connected to  the server a lot easier.
The second option is a nas like truenas which solves the a lot of the security problems and the nas will be it's own server so processing power can be replaced with extra storage meaning more storage for the same cost this also allows for use of things like raidZ3 to keep all data safe even with multiple hard drive failures which is good for the user truenas also offers a web interface once set up which is very useful for setting up user permission and drives .

So seeing this the this the obvious choice for the end user is to use a nas like truenas but at first I write this off as all the equipment I was given was a windows machine and thought if I had the hardware I would use a nas but then I resized I could use a virtual machine to simulate using truenas which worked quite well
This gives extra file safety, security, and storage for shared files while in this example it's a vm with 8GB of ram and 50GB of storage a simple nas could easily be made of $500 that has a few terabytes and 8 to 16GB of ram as the cpu does not need to be that great and more storage can be added and this would be plenty for a house hold and if they need more they could spend more money. this also means that more computers can connect without worrying in the example of the simple FTP server if the windows machine is on or not for example windows updates 

since I was using computers at the school the hardware choice was limited for actually making it eg I used a vm for TrueNAS. But if I had the choice I would have 8 to 16GB of ram depending on how much storage is needed as ram helps with accessing it, a few taryebytes of storage probably 2-4 for most familys but more if needed and mid ranged cpu like a ryzen 5600 if top preformance was needed something like an epic would be used but thatnot need for a few TB of storage and a gpu wouldn't be needed unless truenas was using a app that was gpu entensive for some sore of security calculation but this would be unlikely though some sort of graphics would be needed for the orgininal setup integrated graphics would work perfectly fine for this or some really old gpu as well as it's just redering text  this would cost around 500-700 dollaors depending on specifics and prices at the time. 




#### windows FTP
![[orignial fileshare.png]]
the widows FTP worked but required a lot of changeing of settings and configuring the windows fire wall to allow filesharing while this worked but lacked security, file saftey and uses local storage on the computer for all files which makes it a little impractical esspecally with a nas(network-attached storage) as the allternativewhich is better at all the point above  


#### TrueNAS Running
![[virtual truenas.png]]
after a simple instalation process TrueNAS was running as a vm(virtual machine) though changes a little bit how the physical network works but it works pretty much the same but inside the computer and the ipV4 address can be seen 10.160.135.22 and at port 80 a http server and at port 433 a https server these are the defualt ports for http and https packets to come from 
 
#### Trouble shooting
while setting up the TrueNAS when mostly smoothly I did run into some problems at the start because I hadn't set up SMB sharing to work on the drive this was pretty simple just adding it to the drive. I also had to set up the user with presmissons to read and write to fit the porpose of the file server 

#### windows machine(1)
![[micah.png]]
this is from the windows machine(1) showing the folder that is shared for the user micah on the truenas server 


#### windows machine(2)
![[file-share-fileexsplorer.png]]
this is from the windows machine(2) showing the folder that is shared for the user micah on the truenas server and has the same new folder as windows machine(1)

---

## [[OSI]] Model  
### Physical Layer (Layer 1):
At the Physical Layer, the network uses Ethernet (IEEE 802.3) as the physical medium or the virtualised equivalent, typically with twisted-pair copper cables or research optic cables. The devices, such as Windows Machine(1)  , the TrueNAS server, and Windows Machine(2) , are physically connected to the switch using Ethernet cables. this layer is the simplest.

### Data Link Layer (Layer 2):
The Data Link Layer is where devices are identified by their unique Media Access Control ([[MAC]]) addresses. When Windows Machine(1) communicates with the TrueNAS server or Windows Machine(2), it uses MAC addresses to ensure data is sent to the correct destination this does have its flaws. The switch maintains a MAC address table that associates MAC addresses with the corresponding switch ports. This allows the switch to forward data frames directly to the intended recipient.

### Network Layer (Layer 3):
At the Network Layer, the network uses the Internet Protocol version 4 (IPv4) or (IPv6 works pretty much the same but allows for more adresses) for addressing and routing. Each device in the network has an IPv4 and IPv6 address, allowing them to communicate with each other. For instance, the TrueNAS server has the IP address 10.160.135.22, Windows Machine(1) could have 10.160.141.93, and Windows Machine(2) could have 10.160.141.94

When Windows Machine(1) wants to access files on the TrueNAS server, it first needs to know the MAC address associated with the IP address of the TrueNAS server. It does this by sending an Address Resolution Protocol (ARP) request to the local network. The ARP request asks, "Who has IP address 10.160.135.22?" The TrueNAS server replies with its MAC address, and Windows Machine(1) stores this information in its ARP cache. 
This layer is also where defualt ports for protocols are from eg. 80 is http 443 is https and 21 is for ftp this is how it is generaly known along with the type of packets what protocols to use for which data that come through the phsyicall layer 

### Transport Layer (Layer 4):
At the Transport Layer, the network primarily uses the Transmission Control Protocol (TCP) for reliable data transfer. When Windows Machine(1) wants to share a file with the TrueNAS server, it establishes a TCP connection with the server to ensure data integrity.

For example, let's say Windows Machine(1) uses the File Explorer to copy a file to the TrueNAS server. The following steps occur:
    Windows Machine(1) sends a TCP SYN packet to the TrueNAS server to initiate a connection.
    The TrueNAS server responds witha TCP SYN-ACK packet, acknowledging the request.
    Windows Machine(1) sends an ACK packet to complete the three-way handshake, and the connection is established.
    this is how most things on the internet work, using TCP.
    Windows Machine(1) starts sending the file data to the TrueNAS server in TCP segments.
    The TrueNAS server acknowledges the receipt of each segment, and if any segment is lost or corrupted during transmission, Windows Machine(1) retransmits it.
    Once the file transfer is complete, the connection is closed with a TCP FIN packet.

### Session Layer (Layer 5):
The Session Layer is responsible for establishing, maintaining, and terminating sessions between applications on different devices. It ensures that data exchange between applications is properly synchronized. So that each packet isn't blindly sent.

In the file-sharing network, the Session Layer might not be heavily utilized for simple file sharing between the two Windows machines and the TrueNAS server. The file sharing process can be viewed as a "session" in the context of the OSI model, where the session begins when Windows Machine(1) establishes a connection to the TrueNAS server and ends when the file transfer is completed and the connection is closed. this is how the whole file gets transferd in little packets while staying together. kinda there isn't always a session except when files are being copyed across machines 


### Presentation Layer (Layer 6):
The Presentation Layer handles data formatting, compression, and encryption to ensure that data from the application layer of one device can be understood by the application layer of another device. this is important for security as well.

In the context of file sharing, the Presentation Layer is involved in data conversion or encryption if specific requirements exist. For example:
    Data Formatting: If Windows Machine(1) sends a file in a specific format (e.g., ASCII text), the Presentation Layer may need to convert it to a different format (e.g., Unicode) for the TrueNAS server to understand it correctly. the same would be true for all file type extensions 
Data Compression: The Presentation Layer can compress the data before transmission to reduce the file size and optimize bandwidth usage.
	Data Encryption: If security is a concern, the Presentation Layer can encrypt the file data to ensure confidentiality during transmission. the TrueNAS can do this customizably and was one of the reasons it was picked 



### Application Layer (Layer 7):
At the Application Layer, the network uses the following protocols for file sharing:

Server Message Block (SMB) / Common Internet File System (CIFS):
SMB is a network file-sharing protocol primarily used by Windows devices to access files, printers, and other resources on remote servers. NFS would be used if a unix computer was used on the network like mac or ubuntu but since windows was used SMB is used.Note that SMB can work with mac as well some setting have to be changed though because SMB has some vulnabilitys. When Windows Machine(1) wants to access shared files on the TrueNAS server, it establishes a session with the server using SMB. SMB allows Windows Machine(1) to send commands and requests to the TrueNAS server to read, write, and manage files. The TrueNAS server responds to these requests and grants access to the shared resources based on user credentials ie if the user can write , read or read and write.

Credentials: Windows Machine(1) provides its TrueNAS username and password as credentials when connecting to the TrueNAS server. The TrueNAS server validates these credentials to determine whether the user has the necessary permissions to access the shared files and if they can write or just read.

Network File System (NFS):
NFS is a file-sharing protocol commonly used in Unix-like operating systems and works slightly differently.

Credentials: To access the NFS shares, Windows Machine(1) needs to provide appropriate NFS credentials, which may include a username and password or a secure NFS client configuration.


### Relevant implications
#### security
security is important for home systems as they can hold lots of personal data that would be detrimental to lose. using TrueNAS helps increase the security with easy control to what users can access each folders meaning that less trusted people can have read access. white listing can also be used to only allow certen MAC adress or IP adresses though these can still be spoffed it still requires a knowledge of the white listed MAC adresses and IP's encryption can also be enabled which is important for sensetive files because even if there accessed they can't be read or writen to in any meaningful way it is important to note that most security stuff do have there down sides in some way of preformance or another way but this wouldn't be really noticebel in this situation  

#### sustainability and furture proofing 
for anything used in daily life like this file server it is important for it to work well into the furture. as for the networking part of everything is very backwards compatable so shouldn't have anything major to change to stop the nas from working with computers same with all the protocols. this means that this setup should work in 10-20 because of backwards compatablity. There is one flaw to this though even though it would work it would be at the detrerment of security as things will stop getting updated. this means that the system should be updated regularly to keep up security. as hackers are always finding ways to get past systems so this is unavoidable.

on the other side of thing truenas provieds a way to easily set up something like raidZ3 which keeps all files safe even if a drive fails for whatever reason which means unless the whole server gets destroyed all the files should be safe. as hard drives tend to fail after a while.

#### end user requirements
meetting the end user requirements is quite important for any project as this is the main purpose for it. there are two main end user those who are not going to set up the server and would have someone set it up for them so only the use is important but there is also those setting it up them selves. For just the use it is pretty simple and easy to use just having a folder with shared files that can be accesed with a username and password which is about as simple as it can get for folder and file management. and even in the second senerio setting up the server is pretty simple just by using defualts selecting a drive turning on SMB share and createing a user which is much simpler to setup than digging through windows control pannel or config files. which is very helpful for the end user.










