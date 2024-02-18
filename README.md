# Training exercise 1 - Networking

The questions serve to ensure that mere basic understanding of what networking
is and how it works. 

## A Network - The basics

What is a _network_?  Discuss what an IP address and associated netmask
respresent in a network and why these are important.
- How does one come from one network to another?
  > A network is a collection of interconnected devices or nodes that can communicate with each other.
  > An IP address, along with its associated netmask, identifies a device on a network and determines the range of IP addresses that device can communicate with. IP addresses are crucial for routing data packets across networks.

## Addresses

IP addresses can be designated either statically and dynamically. 

### Static IPs

Why would you use _static IPs_, what do you think would be the point?
Two things:
  > Static IPs are used when a device needs a permanent IP address that doesn't change over time.

- How is this controlled?
- Where are they set / configured?
  > They are controlled by manually assigning a specific IP address to a device and configuring it in the device's network settings.


### Dynamic IPs

Explain what happens in situation a server/desktop/node is to have a dynamic IP. 
- What happens?
  > In a dynamic IP scenario, a server/desktop/node is assigned an IP address dynamically by a DHCP (Dynamic Host Configuration Protocol) server.
- Who is responsible for what?
  > The DHCP server is responsible for leasing IP addresses to devices on the network, managing IP address allocation, and ensuring no conflicts occur.


## DNS

DNS is used on a daily basis and actually constantly. 
- For what and where?
  > DNS (Domain Name System) translates domain names to IP addresses, allowing users to access websites using easy-to-remember names instead of numerical IP addresses.
- Explain what happens when tcp connection is to be made to a server from a DNS
  point of view.
  > When establishing a TCP connection to a server, the client queries a DNS server to resolve the domain name to an IP address before initiating the connection.

## Router/Firewall

Whats does one do and how?
>Routers forward data packets between different networks, while firewalls regulate incoming and outgoing network traffic based on predefined security rules

## DNAT, SNAT, PAT & Port forwarding

Explain these concepts and what purpose they fulfill.
- Which of these are denoted in a _Dockerfile_ and how is this shown?
  > - $DNAT$ (Destination Network Address Translation) and SNAT (Source Network Address Translation) are techniques used in NAT (Network Address Translation) to modify IP addresses and port numbers in packet headers for routing purposes.
  > - $PAT$ (Port Address Translation) is a type of NAT where multiple private IP addresses are mapped to a single public IP address using different port numbers.
  > - $Port forwarding$ redirects traffic from a specific port on the router's public IP address to a specific port on an internal network device.
- This functionality is important why?
  >- DNAT helps direct incoming messages to the right place inside your network
  >- SNAT helps direct outgoing messages to the right place outside your network
  >- PAT allows multiple devices to share a single public IP address
   
# Training exercise 2 - Composing

## Preassumption

To get started with the exercise one should have system setup consisting of
multiple containers that work in concert.

The focus of this exercise is getting a _docker-compose_ working with a _NodeJS_
application. Icing on the cake for this exercise is enabling and testing
debugging for said application.

It is presumed that you have a working _Dockerfile_ that can be build and
run your application.

Completing the same exercises for a C# .NET application is left as a home
exercise. For this part you can use
https://gitlab.au.dk/swwao/demo-projs/debugging-net-core as a starting point.

## Create your first compose 

In this first task create a compose file that has 2 services:
1. Your application based on the aforementioned _Dockerfile_
2. The associated database

Think about how the networking setup actually is. 
- Where is the host (our own host)
- Where are the 2 services placed in the network
- How do they accessible from the real world
- How do they contact each other, how is this controlled

Draw a simple diagram depicting the network topology. This should include the
needed port forwarding!

Having done all that verify that your application indeed has a connection to the
database and that data in the database is actually changed.

## Adding packages to your app

Add a service which only task is to run _npm_ in an identical environment as of
that of our application. The point being that if we at some point need to use
addtional packages or update one, this has to be done in complete controlled
environment as when the application is run.

- How will you go about this task?
- Which image will you?
- Bind mounts?!

Running this service should be run without any others running at the same
time. Conversely running up to start the application and database should not
start this as well. To ensure this utilise
_profiles_. https://docs.docker.com/compose/profiles/

The development environment that has to far been created could be named the
_debug_ profile and running _npm_ could be named the _script_ profile.

Do figure out how you run a services as well as how you utilise the profiles
from the commandline.

Implement and test...


## Live reloads

Creating the aforementioned _Dockerfile_ meant that the sources for the
application is and will be "baked" into the image. For creating the distribution
files thats fine, however during the development phase rebuilding the image at
every file change will simple be too time-consuming.

The task is thus to ensure that the source files are _bind mounted_ into the
container and that _nodemon_ is used to watch the files, such it reloads when a
source file is changed.

## Database volume

If nothing specifically has been done, then data in the database will be lost
between a _docker compose down ..._ and docker compose up ..._. This obviously needs to
be fixed.

Try two different solutions namely _bind mount_ and _named volume_. In the
latter case specify the volume as external and create volume separately such
that its livetime exceeds the lifetime of the database container.

Perform adequate tests to these two approaches...



## Development & Production environment

Add yet another service and corresponding _production_ profile to run the system
in a production setting.

Verify that the application is actually prepared for production mode and then
started as such.


## Environment variables - Bonus

In the described setup it is presumed that the connection string to the DB is
either hardcoded and/or without a user+password. This should obviously not be
hardcoded, therefore add environment variables that contain the necessarily
information. Since this information is sensitive place these in an environment
variable file that is loaded in the _docker-compose_ file.


# Training exercise 3 - Debugging 
    
## Debugging a Nodejs application

Take your starting point in debugging your node application from the previous
lectures. If you want something very simple, then use the demo project available
here: https://gitlab.au.dk/swwao/demo-projs/node-demo Folow the instructions in
the video https://www.youtube.com/watch?v=ktvgr9VZ4dc and get acquainted with
debugging.

There are no special requirements other than training and ensuring that
debugging process works comptable.

For a complete example see: https://gitlab.au.dk/swwao/demo-projs/typescript-mongodb-crud

## Debugging a C# application - For Home

For this exercise it is assumed that you have a C# .NET application.  Complete the
appropriate tasks associated with debugging a C# .NET in a container - similar to Nodejs.

Follow guide: https://code.visualstudio.com/docs/containers/quickstart-aspnet-core

There are no special requirements other than training and ensuring that
debugging process works comptable.
