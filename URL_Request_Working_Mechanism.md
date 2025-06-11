# "What Happens When You Type 'google.com' And Press 'Enter' "
## The Working of URL Requests and Response on the Browser

If you have ever searched for something on your browser, say "https://youtube.com/", then you may need to know ___what happens in the background for your site to be loaded.___
__Let's Dive In:__

### What happens when you type "google.com" on your browser and hit 'enter': 

__Essential Terms in this blog:__
__Browser__: the application that let's you send HTTPS requests e.g. Chrome, Safari, Firefox and Ms Edge
__Client__: Your computer that sends the URL request when you press enter
__Server__: The computer that contains the resource that you are requesting; it responds to the client with the requested resource or an error in case there is a failure 
__Request__: The URL that you / the client sends e.g. "google.com"
__Response__: the Webpage that is given back as a response
__HTTPS__: Hypertext transfer Protocol Secure
__URL__: Universal Resource Locator 
__TCP/IP__: Transmission Control Protocol / Internet Protocol
__DNS__: Domain Name System Server 

A series of steps takes place in those few seconds before you get a response, these step are: 

1. The domain name system (DNS) server, acts as an address book for all domain names. It receives a request from your computer and returns and IP address of the server where the resource (https://www.google.com) is found.

1. Your computer then connects to the server using the IP address once it knows this IP.  Your computer can create this connection to the Server, this connection is called Transmission Control Protocol (TCP), using the Internet Protocol (IP).  This entire procedure is called a "handshake".
1. If a firewall is installed on your computer, it will first verify that the specific request you are making is permitted before allowing it.  Additionally, if the server you're attempting to reach is likewise protected by a firewall, it will carry out a similar verification before responding to your request.

1. To secure the data that will be transferred between your machine and the server, your browser now uses an encryption protocol, such as Secure Sockets Layer (SSL) or Transport Layer Security (TLS), to send a request for the webpage after the connection has been established.  The "s" in "https"—which also indicates that the connection is secure—is caused by this kind of encryption.

1. In addition to maintaining a large number of servers, high-traffic companies like Google also have a load balancer that takes in the majority of requests and routes them to a specific server.

1. Following receipt of the request, the load balancer receives a response from the server and relays it to your browser.  The HTML, CSS, and JavaScript files that make up Google's homepage will be the main components of this answer.

1. The HTML files were returned to instruct the browser on how to display the page's content.  While the JavaScript file gives the page interaction, the CSS file instructs the browser how to style the content.

1. The web server will send a request to the application server, which may then send a request to a database server to retrieve some data and return it to the web server, if dynamic content, like Google search results, is required.  The web server will then include these in the response it sends back to the browser.

1. Finally, the browser will render the page and display it to you.

## A deep dive into the steps above:
## DNS Request (Domain Name System)

- When you enter the search google.com into your browser, the browser checks if it already has the website’s IP address stored in its cache. If it does, it uses that to contact the server directly, saving time.

If not, it goes through a process to find the IP address. This process is called DNS lookup, and it involves several steps:

The browser asks the local DNS resolver (usually your ISP) if it knows the IP address.

If not, the resolver contacts a root DNS server to find out which Top-Level Domain (TLD) server (like .com) to ask.

The resolver then asks the TLD server, which points it to the authoritative name server for the domain.

The authoritative server gives the actual IP address.

The resolver returns the IP to the browser, which then contacts the website’s server.

This IP address is saved (cached) for a certain time, defined by the TTL (Time To Live) value set by the domain owner.

## TCP/IP Connection
The Internet runs on two main protocols: TCP (Transmission Control Protocol) and IP (Internet Protocol).

When you visit a website:

Your browser uses IP to locate the server.

Then, TCP creates a connection using a process called a handshake.

After the handshake, your browser sends a request to load the page.

The server sends back the webpage data using TCP, ensuring that everything arrives in the right order.

Your browser receives and displays the webpage.

## Firewall
A firewall is a security tool that monitors traffic between your device and the internet.

When your request to access google.com is made:

The firewall checks whether the request is allowed, based on preset rules.

These rules might block traffic from certain regions, or only allow certain types of traffic (e.g., only HTTPS).

If your request matches the firewall’s rules, it’s allowed through. Otherwise, it’s blocked.

HTTPS and SSL/TLS
HTTPS is a secure version of HTTP. It encrypts your data to protect it from being read by outsiders.

The encryption is handled using protocols like SSL (Secure Sockets Layer) or TLS (Transport Layer Security).

Analogy: Imagine sending a message in a locked box. Only the intended recipient has the key. SSL/TLS are the locks and keys, while HTTPS is the secure box itself.

When you connect to google.com, your browser and the server agree on how to encrypt data, so everything sent is private and secure.

## Load Balancer
A load balancer spreads incoming traffic across multiple servers to ensure no single server gets overloaded.

When many users try to access google.com, the load balancer distributes those requests across Google’s many servers. This keeps things fast and stable.

## Web Server
A web server handles requests for web content like HTML, CSS, and JavaScript.

Once the load balancer chooses a server:

That server receives the browser’s request.

It gathers the necessary files.

It sends them back through the load balancer to your browser.

## Application Server and Database
While web servers deal with static files, application servers handle dynamic content.

For example:

When you perform a search on Google, the request goes to an application server.

That server processes your query and may contact a database to get results.

It then sends the data back to the web server, which returns it to your browser.

## Rendering the Page
Once the browser receives everything:

It reads and processes the HTML, CSS, and JavaScript.

It places text and images where they should go.

It applies styles and runs scripts.

You can now interact with the page—click links, type, etc.

