🐳 Docker Bridge Network – Explained Clearly

While learning IP addressing and subnetting, I observed that Docker automatically creates a default network called:

bridge

This helped me understand how CIDR and private IP addressing are applied in real DevOps environments.

Let’s understand this clearly.

What is Docker Bridge Network?

When Docker is installed, it automatically creates a virtual network inside the host machine.

This default network is called:

bridge

It acts like a small private network inside our server.

All containers launched without specifying a network are connected to this bridge.


Where Does It Exist?

The bridge network exists inside the host machine.

Docker creates:

   - A virtual network interface called docker0

   - Assigns it an IP like 172.17.0.1/16

   - Connects containers to this interface

You can verify this using:

    * ip addr

You will see:

docker0 → 172.17.0.1/16

This confirms Docker is using CIDR internally.

Why 172.17.0.0/16?

Docker uses a private IP range.

Private IP ranges are:

  - 10.0.0.0/8

  - 172.16.0.0/12

  - 192.168.0.0/16

These are defined by:

Internet Assigned Numbers Authority

Docker selects 172.17.0.0/16 from this private range.

Let’s break it:

172.17.0.0 → Network address
/16 → First 16 bits are network

Host bits = 32 - 16 = 16

Total possible IPs:

2^16 = 65,536

So Docker bridge network can theoretically support 65,536 container IPs.


How Containers Get IP Addresses

When I run:

    docker run -d nginx

Docker assigns container an IP like:

   172.17.0.2

Another container may get:

   172.17.0.3

These IPs are assigned from:

   172.17.0.0/16

This is real-world usage of CIDR.

How Containers Communicate

If two containers are in same bridge network:

   - Container A → 172.17.0.2

   - Container B → 172.17.0.3

They communicate directly because:

   - Same network

   - Same subnet

   - Same CIDR

This matches our earlier concept:

Devices in same subnet can communicate directly.

Why We Cannot Access 172.17.x.x From Outside?

Because 172.17.x.x is private IP range.

Private IPs are not routable on the internet.

So Docker uses:

NAT (Network Address Translation)

When I run:

docker run -p 8080:80 nginx

It means:

    Host port 8080

    Container port 80

Traffic flow:

User → HostIP:8080 → Docker NAT → 172.17.0.x:80

Docker translates traffic between public host network and private container network.

This is practical implementation of NAT concept.

When Problems Can Happen

    Understanding this helps when:

    - Containers cannot talk to each other

    - Port mapping is not working

    - Server already uses 172.17.x.x range

    - Network conflicts occur

If host network already uses 172.17.0.0/16,
Docker bridge may conflict.

In that case, we create custom network:

docker network create --subnet 10.10.0.0/16 mynetwork

Now we explicitly control CIDR.


Final Understanding

Docker bridge network is a practical example of:

   Private IP addressing

   CIDR usage

   Subnetting

   NAT

   Network isolation

This helped me connect pure networking theory with real DevOps implementation.
    


