#### Nano Hackers Extra Session 4

# Networking

> We ought to connect all these computers. —Bob Taylor, Director of ARPANET

## Internet Layer

### IP Addresses

What's this doing?

```
$ ping 131.215.239.141
```

It's sending a tiny bit of data—a _packet_—to another computer. That computer is then sending a packet back. How long does each packet exchange take?

```
$ geo 131.215.239.141
... Pasadena ...
```

This computer is in California. How fast is the data moving?

What's `131.215.239.141`?

It's an _IP address_. Every computer on the Internet has an IP address. What's the IP address of your computer?

```
$ ifconfig
```

**Lab**: Let's ping each other's computers. First make sure your _firewall_ is allowing pings. You can turn off the firewall altogether (in System Preferences) or disable "stealth mode."

### Packets

What exactly is being sent between our computers? Let's take a look.

We're going to use a program called _Wireshark_. Wireshark lets us examine all the traffic going to and from our computer.

```
$ wireshark
```

Woah! There's a lot of network traffic going through our computer. Let's filter the capture to just ping (aka "ICMP").

Both packets—request and reply—contain a timestamp. This is the time the packet was created. Given this information, how does the _ping_ program calculate the round-trip time?

**Lab**: Install Wireshark and play with packet captures. What happens when you go to a web site in your browser?

### Routing (Optional)

Wireshark shows us that packet contains a _source address_ and a _destination address_. These tell the Internet where to _route_ our packet.

How do you think the Internet routes packets?

Let's take a look at how a packet gets from our computer to `8.8.4.4`:

```
$ traceroute -n -w 1 8.8.4.4
```

## Transport Layer

We know how bits of data are sent (packets!) but what if we want to send bigger things—files, videos, images?

We divide them into packets. So, let's do just that. I'd like to send a list of the numbers from 1 to 10 to a server I've sent up, `nano.jonahb.com`. I'll send each one in a packet.

First, I'm going to start a program on the server that will print whatever's sent to it. (We'll learn about `ssh` later.)

```
local$ ssh root@104.236.16.171
remote$ ncat -u -l -p 3000
```

Just to illustrate, I'll connect my local computer to my server:

```
local$ ncat -u 104.236.16.171 3000
```

Now, I've written a program that sends the numbers 1 to 10 to a server:

```
remote$ ncat -u -l -p 3000
local$ ./send_numbers 104.236.16.171 3000
```

What happens? The numbers appear out of order! The Internet is a big place, and some packets take longer to get to the destination than others. Think of it as a highway system with cars taking differen routes on the way to a common destination. In fact, the Internet doesn't guarantee that packets will be delivered at all. How might we go about fixing this?

What if we added numbers to the packets? That's exactly what the inventors of the Internet did, and modern computers come with software that automatically adds these numbers to your data. This software is called a _TCP_. Let's try a version of our program that uses TCP.

```
remote$ ncat -l -p 3000
local$ ./send_numbers_tcp 104.236.16.171 3000
```

Ah, now the data arrives in order. If we look at the transmission in Wireshark, we see the sequence numbers.

Now that we can send data reliably, we can create some interesting servers:

```
ncat -l 3000 --exec "/usr/bin/xargs -L 1 cowsay"
```

Can someone connect to my computer with `ncat`?

**Lab:** Find a partner and experiment with `say` and `figlet` in place of `cowsay`.

## Application Layer

### Names

So far we've been typing IP addresses. But we don't type these when we're using the Internet every day, do we? Instead we use _names_. What are some examples of names?

In the end, computers are addressed by IP address, so there must be a way of translating names into addresses. This way is called the _Domain Name System_ or the _DNS_.

The DNS is a network of servers out on the Internet. We can query it with the command `dig`. Let's give it a try.

```
$ dig google.com
```

If we type the IP address into our browser, we indeed go to Google. If we run Wireshark and go to `google.com` in our browser, we see the DNS lookup. Our browser—all the programs on our computer–use the DNS the translate names into IP addresses.

### The Web

When technical folks say "the Web," they usually mean two things: _HTML_ and _HTTP_. We know what HTML is. What's HTTP?

HTTP is the _protocol_ that our web browser uses to fetch things from the Internet: HTML pages, images, scripts. HTTP is quite simple. We open a TCP connection to a server and tell it what we want to _GET_:

```
$ ncat jonahburke.com 80
GET /about HTTP/1.1
Host: jonahburke.com

```

Back comes the HTML. This is exactly what your browser does when you type `jonahburke.com/about` into the address bar.

Of course, we don't usually type HTTP into ncat. Instead we use a browser or another program that "speaks" HTTP. One handy one is curl:

```
$ curl jonahburke.com/about
$ curl "http://api.flickr.com/services/feeds/photos_public.gne?tags=cow&format=json"
```

Another is wget:

```
$ wget jonahburke.com/assets/malaren.jpg
$ wget code.jquery.com/jquery-2.2.0.js
```

### Remote Shell

We often have to set up and configure computers on the Internet. To do this, we use a _remote shell_. A remote shell let us run commands on another computer:

```
$ uname -a
Darwin ...
$ ssh root@nano.jonahb.com "uname -a"
Linux ...
```

And operate other computers from our Terminal:

```
$ ssh root@nano.jonahb.com
```

Let's use ssh to set up a web server on `nano.jonahb.com`:

```
$ ssh root@nano.jonahb.com
$ aptitude install webfs
$ mkdir website
$ cd website
$ echo "Hey nanos!" >index.html
$ webfsd -p 80 -F -d -f index.html
```

Go to `http://nano.jonahb.com` in your browser. What do you see?

**Lab:** Mac OS supports ssh too. Turn it on by checking the _Remote Login_ box in System Preferences/Sharing. ssh into your computer from a partner's computer and poke around.

### Homework

Explore dweet.io. Use curl to create a dweet for a thing, read the dweets for a thing, lock a thing, and unlock a thing.
