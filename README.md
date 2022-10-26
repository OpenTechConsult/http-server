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

## Parameterized URLs

The code that we've written so far, which responds to two different endpoints is functional but not very realistic.

When writing real life servers, we will be sending back data that coming back from some sort of database where we might have dozens of different friends. And each of those friends might have their own set of friends.

In our server, we need to have the ability **to query for individual item in our collection** to be able to run queries and get the data that we need to display whatever it is that we're showing to the user.

So when our data lives in a larger collection of data (e.g. a list of all our friends which is an array of many of json type like so)

```js
    const friends = [
        {
            id: 0,
            name: 'Nikola Tesla',
        },
        {
            id: 1,
            name: 'Sir Isaac Newton'
        },
        {
            id: 2,
            name: 'Albert Einstein'
        },
    ]
```

### How do we query that data in the browser?

We will pass in next to the name of the collection, the **id** of the friend that we need to get. So for the data _Sir Isaac Newton_, it will be the **id** number 1, or for _Nikola Tesla_, it will friend at id 0:

```bash
    http://localhost:3000/friends/0
    http://localhost:3000/friends/1
```

The **id** of the friend is a parameter in our endpoint. So we'll often hear this URL a **parameterized endpoint**, or a **parameterized route**, where route is basically another way of saying endpoint. But we'll get into that more when we'll talk about **_routers_** and how they can help us organize our endpoints.

### So how do we do this in our Node.js code?

When we check our request URL, we need to do some sort of additional parsing. There is many ways that we can do this, but a quick one is to create an items list where we split the url that comes in (`req.url`) and we'll call the string `split` method (because `req.url` is a string) splitting it whenever we found a forward slash ('/')

`const items = req.url.split('/')`

Say we have a URL for our request which look like:

/friends/2 . That will gives us an item array of an empty string because that is the first thing prior to the first forward slash, then the 'friends' string followed by 2

> /friends/2 => ['', 'friends', 2]

Now, rather checking the request URL for /friends or /messages, we can check the second item of the items array. And rather than checking if it matches '/friends', we check to see if it matches the 'friends' string. We do the same exercise to the `/messages` endpoint.

Let's run the code to see if it is working as expected.

Now to check that the URL is a parameterized endpoint, we'll check if the length of the array is equal to 3. If it is, we'll send back the stringify version of a friend at the specific index. Since splitting a string gives an array of string, if we wan to get the item at the 3rd index, we need to convert it to a number.

```js
    const http = require('http')

const PORT = 3000

const friends = [
    {
        id: 0,
        name: 'Nikola Tesla',
    },
    {
        id: 1,
        name: 'Sir Isaac Newton'
    },
    {
        id: 2,
        name: 'Albert Einstein'
    }
]

const server = http.createServer((req, res) => {
    const items = req.url.split('/')
    if (items[1] === 'friends') {
        res.writeHead(200, {
            'Content-Type': 'application/json'
        })
        if (items.length === 3) {
            const friendIndex = +items[2]
            res.end(JSON.stringify(friends[friendIndex]))
        } else {
            res.end(JSON.stringify(friends))
        }
    } else if (items[1] === 'messages') {
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

server.listen(PORT, () => {
    console.log(`Listening on ${PORT}`)
})
```

If we test the code we can see that it works as expected.

At this stage, our code is far from perfect and we don't recommend creating code like this in production. For example if we try to get an index that is not in our friend array. Let say we want to access this endpoint

> http://localhost:3000/friends/4

The server does not respond. The server should probably be telling us **404 Not Found** because the friend with `id` oes not exist. And we would'nt go around writing a larger server where we are hard coding the length of our URL, or the amount of parameters that we're passing in. This is rather a proof of concept. What we have demonstrated though is how even simple requests can start getting quite complicated when you need to parse the incoming request. For example to get parameters. Of course, we can improve our code here. But there needs to be a better way. This seems like code that could be written once really well as a package and reuse in most servers that deals with collections of data, like our friends.

As a developer, it is reasonable to expect to have existing packages help us with these common scenarios. That feeling as a developer that a problem has been solve before is a very healthy sign. It's a sign that you might have to search an existing solution, and either use that existing solution or learn from it, so we don't spend time re-inventing the wheel where really good solutions already exist. We can instead spend time resolving problems that make our application unique. Of course when learning and exploring Node.js like we are, it often pays to look behind the scenes like we've done here. But we'll look at some frameworks that will make our life easier very soon.

For now, let's finish exploring Node.js built-in way of working with HTTP. There is a couple more interesting things to learn from how Node.js does things.

## Same Origin Policy

Welcome back!!

Before we move on, we need to understand something important. Something that affects us every when we browse the web. What is it? It is the **Same Origin Policy**. This is something that will come up in our carrier as a Node.js developer. Let's take sometimes to understand this often misunderstood concept.
Let start with what **Origin** is?

When we browse the web and go to a site lik maps.google.com what we type in the browser is something like:

> https://www.google.com/maps

The **Origin** is a combination of three things that we see here.

- **The first thing is the protocol** (_https_): which indicate how we communicate with the server at google.
- Next up, a critical part of the Origin is the **Host**: the host tells us what server we are going to be browsing to, or which server is going to be handling our request.
- The third part of the Origin is the **PORT**: whenever included in our request. The default **PORT** when using _https_ is port 443. When using _http_ the default is port 80.

Whenever one of these three things changes, we are no longer on the same origin. We can browse to other pages at that origin. For example go from the _https://www.google.com/maps_ to _http://www.google.com/mail_ to get our emails. But we can change _www.google.com_ to _facebook.com_ and still be at the same origin. That makes a lot of sens.

But we also can change the protocol. So if we go to _http://www.google.com_ and not _https://www.google.com_ the origin has still changed. The origin is the combination of these three things.

Now why is any of these matters to us? Well, it because our browser and JavaScript use what's called the **"Same Origin Policy"**.

The **"Same Origin Policy"** is a security feature by the browser that restricts what the browser is allowed to load when browsing pages on the Internet. We might be browsing on the computer a site like www.google.com and making requests to google servers. But the JavaScript the is running on the browser that has google's code might also potentially makes request to facebook.com. Maybe it's submitting a form to facebook or getting some data about friends. Under the **same origin policy** the browser allows requests from the same origin as the page that is first loaded in the browser. So we can be on _https://www.google.com_ and make requests to _https://www.google.com_ with the _https_ protocol. But the browser restricts request from different origin than the site that we are currently browsing. So the scenario where google.com get data about friends in facebook isn't allowed. Because that helps protect your privacy.

Say we type in _www.google.com_ in our browser, and we are browsing google. A request to facebook will be denied by the browser, because it is enforcing the "Same Origin Policy".

Now there is an exception. Writes that cross origin servers, so from google.com to facebook.com are often allowed, even with the same origin policy. Writes include things like POST requests, where we submit some data to the server. These are allowed because they don't risk your data from **facebook** being leaked to **google**. Our page at google.com **can write** or **send** anything to Facebook, and it's completely up to the facebook server how it responds to that request. The Facebook server knows where the request came from and what it contains. It can choose to completely ignore that request if it look like it doesn't belong. Maybe it's a request from a hacker or some other malicious site.

Here is an example. Say we are on google.com search page, and we are running a search for _machine learning_. Google will recommend me a series of links based on its algorithm. Now we can easily cross origin from **google.com** to wikipedia.org just by clicking on the link referencing **Wikipedia.org** page. We didn't make the request from JavaScript so the **same origin policy** didn't restrict us. Instead we click on a link in the HTML of the **google.com** page.

But if we try to instead get **Wikipedia.org** data from JavaScript, say by going to JavaScript console and using the built-in **fetch()** function that our browser has in the global `window` object. We can use the `window.fetch()` function to pass in the URL of the server we want to fetch data from.

> `window.fetch('https://en.wikipedia.org)`

This is now a **Cross Origin Request** because we're browsing google at the moment and we're trying to get the name page from a different domain. If we run  the program in the console, we get this error that says the following.

```bash
Access to fetch at 'https://search?ei=cx.....ps://en.wikipedia.org/'
from origin 'https://www.google.com' has been blocked by CORS policy: 
No 'Access-Control-Allow-Origin' header is present on the requested resource.xxxxxx
```

So by reading the error message, we need to understand what CORS is. Well there are many rules that control which kind of requests we can make with the **Same Origin Policy**. And we've seen the important one in this section. But it's not a simple policy.

So when we need to talk to different servers, across different domains, it's best to make sure that we use what's called CORS (which our browser just warned us about.)

So what's CORS all about? We'll find out in the next section. But what we need to understand about the "Same Origin Policy" is that the main idea behind it, is to keep users browsing the web secure while also allowing them to make the request they need to browse the Internet and visit websites which aren't trying to steal their data.

### Exercise: Same Origin Policy

Let's test our knowledge of the browser's **Same Origin Policy**. Let's answer three quick questions about the following scenario:

Say, we're browsing a page on www.wikipedia.org. In general; will the following requests succeed or fail ?

1. JavaScript **GET** request to www.bank.com

    This request will **FAIL** because it's clearly a violation of the **Same Origin Policy**.

2. A JavaScript **POST** request to www.bank.com

    This request will **SUCCEED** because it's the exception of the Same Origin Policy. It's a write action.

3. Clicking an HTML link to a video on www.bank.com

    This will **SUCCEED** because it is allowed. It's not a JavaScript code trying to GET data from the bank.com site.

