---
layout: post
title: Using Python SocketServer
author: Patrick Shriwise
category: upcoming
tags: python server 
---


## Attending

- 


## Patrick Shriwise

Graduate student of Paul's at UW-Madison.

## Using Python's SocketServer

Today's presenation will be a brief demo of how to setup a local server, handling get and post requests using the server, and using the server to access classes or modules within python from the server instance. We will be using code from [this repo][repo].

###Python modules needed to follow along

       -python-socketserver (for server)
       -python-simplehttpserver (for request handler, ususally comes with python)
       -python-gspread (for editing google sheets)

##Setting up the server

1. Import the needed modules

2. Create a request handler for this server

3. Create a new threaded server class

4. Starup the server

---
```
#1
import SimpleHTTPServer, SocketServer

#2
handler = SimpleHTTPServer.SimpleHTTPRequestHandler

#3
PORT=4000
httpd = SocketServer.ThreadingTCPServer(("", PORT), handler) # Can also use ForkingTCPServer

#4
print "serving at port", PORT
httpd.serve_forever()
```
---

Start the server by running `python server.py` in the terminal.

You can now access this server by navigating to <http://localhost:4000>, and what you'll see is pretty standard server behavior. There will be a listing of all the files in your directory as well as some navigation options for moving through your file tree. By having this in our own python instance however, we can do more... 

(Note: an easier way to get a simple file server like this from the cmd line is `python -m SimpleHTTPServer <port_number>`)

##Replacing the Request Handler

Rather than relying on a standard request handler, we can create our own for more control:

```
class Handler(SimpleHTTPServer.SimpleHTTPRequestHandler):

    def do_GET(self):
        self.send_response(200)     #  Send 200 OK
        self.end_headers()
        self.wfile.write("Hello Bucky!")
```

And give this to the server instance accordingly

```
httpd = SocketServer.ThreadingTCPServer(("", PORT), Handler)
```
So now when we visit our [server][server], the browser will simply get an error response of "OK" and will display the text "Hello, Bucky!". Pretty simple, but the effect is that we can now return any text we'd like *from the python instance*, including html. (see branch demo2 on at the [repo][repo]) The takeaway is that you can now run a subset of processes locally within the python instance.

## The Tie-in







[server]: https://localhost:4000
[repo]: https://github.com/Pshriwise/thw_server_demo
[code]: {{ site.github.repository_url }}/tree/master/topic