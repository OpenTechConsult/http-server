# http-server

## Introduction to HTTP Response and Requests

We've seen that when you browse to a site, we used this system called DNS to figure out where that site lives, which IP address we should use when communicating with it.

And we can then make **requests** to our HTTP web server to get data back from it. The thing that define how our web server respond to these requests is our **API**. The API tells us what kind of functions the server should support and how those functions should be used. Functions like for example **_getting a list of friends_**, or **_getting their messages_**, or **_getting their photos_**.

We can implement our API on the server in Node.js, in Python, or any other programming language. What matters is that the language that we used when reacting to these requests and responding to them is HTTP, which is the **common way** that the browser and the server can use to understand what both sides are saying. So the browser is speaking HTTP to the server and the server can respond, because the server also speaks HTTP.

### So what does this language look like, what are the words the HTTP uses.

For that we can browse to the Mozilla Developer Network Documentation (MDN) which has this excellent reference for the **HTTP Requests Methods** which are also sometimes called **_HTTP Verbs_**. They represent all of the possible actions that you might want to perform on the server side as browser.

- So a browser might want to **GET** some data or a file stored on that server.
- the browser might want to **POST** some data, which is another of saying _submit_ or _send_ something to the server (a message or maybe a photo).
- or maybe the browser want to **DELETE** some data that the server has about us as user

There are few more methods that HTTP support that are less common to use but we'll go over them when it makes sens to. That's it. We can see on the MDN page, all of the HTTP methods, all the words in the HTTP language that we need to learn to build the app of our dream.

HTTP has become the language of the web because it's so simple. In the next section, let's take a look at the kind of messages that we can send and receive in HTTP using the methods that we've seen here.


## HTTP Requests

What kind of messages do we send back and forth in HTTP? What an HTTP conversation look like? Remember that an API stands for Application Programming Interface. It specifies how two applications talk to each other, for example a browser and a web server. And On the web, it's generally the browser making the requests and the server responding to those requests. If we think of an API, almost like a set of phrases in a language that we can put together to communicate an idea or to get some work done, then it's generally the browser that starts the conversation and asks questions of the server, waiting for it response before potentially making another request. It's almost like the browser interviewing the server.

### So what kind of things can we ask of our web server?

With HTTP, our API is made up of operations using these methods like: GET, POST which we saw that are used on different collections of Data like for example a list of friends (GET /friends) or messages (GET /messages) or photos (GET /photos). We can combine **the name of the method** with the **collection name** to get the data that the server has on who our friends are. Or we can add an Id to the name of our collection to specify that we want to get data about a specific friend (e.g: get data on the friend of id 5, GET /friends/5)

We can generally categorize our requests into requests that are been made on collections of data and requests that are been made on specific item in the same collections. This apply to all of our HTTP verbs. So we can also:

```bash
    POST /messages : if we wanted to save a message and send it to our friends
```

But in general it does not make sens to try to POST to a specific message. Because we  generally only add to collections of data and not to individual items. So the following API phrase is not correct:

```bash
    POST /messages/15
```

But it is possible for us to want to update a specific message to for example change some text or add a photo. And so for that we'll use a **PUT** which allows us to replace the message that we specify with message that we pass in through that request (also called the body of the request)

```bash
    PUT /messages/15
```

We might decide that we are no longer happy with our messages and want to **DELETE** it or maybe we want to remove a specific friend.

```bash
    DELETE /messages/15

    DELETE /friend/5
```

These are possible HTTP requests.

Every one of these request have 4 main parts:

- First is the **METHOD** (GET, POST, PUT, DELETE, PATCH): defines the operations that the browser want to make to the server.
- Each verb is matched up with a **PATH** to a collection or a specific item of a collection. This **PATH** is also sometimes called the **RESOURCE** that we're accessing on the backend.
- We send data to the server by adding a **BODY** to our request which contains the data that we are sending from the browser to the server. This could be in plain text or in a variety of formats. The most common format being JSON. (e.g `{text: "hello", photo: "smile.jpg"}`). Usually we'll have a body on **POST** requests and on **PUT** requests where we are updating data on the server. But we we will not have **BODY** on **GET** or **DELETE** requests, because on those requests the server knows everything it needs to from the METHOD and PATH.
- And last (but not the least), the fourth part of every request which is the **HEADERS**. These are _optional_ properties that we can specify on requests to send additional metadata to the servers. That is _data about the data_ that we're are sending, _data about the request_ (E.g. _**size**_ about the data we're sending', any authentication information we need to send so that the server knows that we're allow to perform that operation with our user). These are _optional_ depending on our use case. But there is one HEADER that every single request needs to have. It is the **HOST** header which specify which server your request is being sent to, including sometimes the **PORT** number for that site. The **HOST** needs to be specify to verify that we're sending our request to the right server.

That's pretty much it. Using these few  parts, we can send over any request we can imagine over to the server so that it can respond to our request.

We'll find all about the response in the next section.

## HTTP Response

We've seen how the browser sends messages or requests to the server by passing:

- the **METHOD**
- the **PATH** to the resource or collection
- sometimes the **BODY** with some data to send to the server
- and some optional list of **HEADERS** with metadata about that request.

### What does the server respond with?

**Responses** in HTTP are even simpler than requests.

With Responses with have three (3) main parts:

  - Like in request, we optional list of **HEADERS**
    - (e.g. "**Content-Type: application/json**") which is a HEADER that we might see both in request and response. It tells us the type of data that is being sent in the body of request or in the body of the response.
  - The second main part of the response is the **BODY** that contains the data that we're fetching from the server. If we set the Content-Type as _application/json_ on the server side, that means that the server is sending a JSON object (e.g. a message from the messages collection like so `{text: "Hi!", photo: "wave.jpg"}`)
- The response also include a **STATUS CODE** which tells us if the request was successful. If not it sends us an **ERROR_CODE** telling us what went wrong. The status code that we want to is **200** which indicates that the response is a successful one. 
  - There are few more possibilities. Every possible HTTP status code can be put into one of the five group
    - Informational responses (100-199)
    - Successful responses (200-299)
    - Redirect errors (300-399)
    - Client errors (400-499)
    - Server errors (500-599)

For a reference documentation on status code see [here](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status)

Before we move on, we can solidify what we learned and look at a request and response in action by heading to the developer console in Google Chrome. From there, go to the Network tab and see the flow of HTTP request and responses activities while browsing a web page.

## Our First Web Server

We're about to reach a big milestone in our knowledge of Node. It's time to making Node servers that we can put on the web to handle request from users.

Users use web browser and mobile applications to talk to our servers. We are going to start in this fresh HTTP server example with a blank index.js

1. Import the Node.js Core HTTP module. This module allows us to make requests, to respond to incoming requests and to work with different type of content.

2. Now let create our server. What we can by calling `http.createServer`. That will give us a server object that we can use to start up a server for example.

3. The `createServer` function expects a callback function which is called the **request listener**. The listener tells the server what to do when it receive a request. The listener function takes two parameters: 
   1. the IncomingMessage called req
   2. the ServerResponse called res that we send back to whoever make that request.

4.What can we do inside the request listener? Inside the _req_ we can look into the **HEADERS** and the **DATA** that are passed in from the client. And using the _res_  object, we can create responses by writing some data and header to send back to that client.

This is where things start coming together, because both the `req` and `res` objects are streams just like what we saw in the planets project.

The request `req` is a readable stream, which we can listen to for data coming in through that stream by using the `.on()` function.

The response `res` is a writable stream and so, say we wanted to respond with a success message. We could use the `res.writeHead()` function to write some headers to the response. To set the data to pass back to the browser, we use the `res.end()` function. It's called _end()_ because it signals that response including the headers and any other data that we want to pass is now complete and ready to be send back. The `end()` function needs to be called on each request that comes into the server. Event if the response is empty.

But we are not done yet. We still need to tell our server to start listening to requests which we can do by calling the `server.listen()` function. Our server will listen by default in the local machine. Our local machine does not have a domain like _google.com_. But we do have an IP address which is usually something like `127.0.0.1` or we could also use the machine default name which is  _localhost_. We can access any server running on our machine by using this _localhost_. What we also needs to pass to the listen function is the PORT number that our server is going to be listening on. Because we can have different applications running on our local machine or on any server and the PORT number is basically used to direct the traffic coming in to the wright server on our machine.

The `listen()` function also accepts a callback function which is run when the server start listening.

If we test the application by running `node index.js` on a terminal window, and if we head to a browser window at the localhost:3000 address, we see that the text _Hello! Sir Isaac Newton is now your friend!_ is displayed which shows that the web server is working as expected.

What if we want to return other types of data. Let's say we want to return a **JSON** instead of a text. Well that's what the **'Content-Type'** is for. Rather than plain text, we can set the **'Content-Type'** to be JSON

```js
    res.writeHead(200, {
        'Content-Type': 'application/json'
    })
```
This allows us now to pass a JavaScript object to the end function like so.

```js
    res.end(JSON.stringify({
        id: 1
        name: 'Sir Isaac Newton'
    }))
```

JSON.stringify() needs to be called because the `end()` function expects a string so this JSON.stringify() function return the string version of the JSON object.

## HTTP API and Routing

Welcome back! Our current web server always responds with the same thing (our friend Isaac Newton). Normally we want our server to be a bit dynamic than this. We want him to respond differently to different type of requests depending on some logic that lives in the server, with different URL representing different types of requests.

Our Node HTTP server that we are creating is an event emitter. When we are passing our callback function to the `http.createServer()` function, remember that we called it the **_request listener_**. That's because when we write this :

```js
    const server = http.createServer((req, res) => {
        // handler function logic
    }))
})
```

it is a shortcut for this :

```js
    server.on('request', (req, res) => {
        // handler function logic
    }))
})
```

However our server isn't giving us different events for request made against different URL.

We can make a request against localhost on port 3000 and passed in any URL like (_mars_) and the very same function that request listener will respond. When we have a server that has multiple different URL that responds differently depending on which one is being requested and with what HTTP methods is being requested, in that case, we call these different URL **endpoints**. This is a term we'll here often when working with servers. Each different URL that we can hit is an endpoint that is responsible for a specific piece of functionality that is provided by the backend server. So what does it look like? How do we make different endpoints for our HTTP server? We do this by looking at the request (`req`) coming in; and specifically by checking for the `req.url` and seeing if that matches the endpoint for our functionality.

```js
    const server = http.createServer((req, res) => {
    if (req.url === '/friends') {
        res.writeHead(200, {
            'Content-Type': 'application/json'
        })
        res.end(JSON.stringify({
            id: 1,
            name: 'Isaac Newton',
        }))
    } else if (req.url === '/messages') {
        res.setHeader('Content-Type', 'text/html')
        res.write('<html>')
        res.write('<body>')
        res.write('<ul>')
        res.write('<li>Hello Isaac</li>')
        res.write('<li>What are your thoughts on astronomy</li>')
        res.write('</ul>')
        res.write('</body>')
        res.write('</html>')
        res.end()
    } else {
        res.statusCode = 404
        res.end()
    }

})
```

Writing out HTML in this way can get pretty tiresome pretty quickly. We will improve upon this greatly. But this demonstrate how we can send different type of contents out from our server and we don't need to set the status code because if we don't send it, it default to 200. We can test the endpoint to confirm it's working.

**What's if we type in some non existing endpoint ?** We don't really get a response. What we should get though is a **404 page**. The good news is that we can this functionality without much code. Our server will tell us that the page does'nt found if none of theses endpoints match. Below is how we write the code for 404 page not found.

```js
    const server = http.createServer((req, res) => {
        if (req.url === '/friends') {
        // ...
        } else if (req.url === '/messages') {
            // ...
        } else {
            res.statusCode = 404
            res.end()
        }
    })
```

We can test the code to see how it behaves.