# Using fetch()

## Learning Goals

- Explain how to fetch data with `fetch()`
- Working around backwards compatibility issues
- Identify examples of the AJAX technique on popular websites

## Setup

This is a code along reading. Please fork and clone this reading down to your
local computer before starting.

## Introduction

When it comes to making engaging web sites, we often find ourselves needing to
send a lot of data (text, images, media, etc.) so that the page is exciting.

**But** browsers won't show anything until they've processed all of that data.
As a result, they show nothing. The screen stays blank and users experience
"waiting."

Too much waiting means visitors will click away and never come back. Web users
expect sites to load quickly **and** to stay updated. Research shows that 40
percent of visitors to a website will leave if the site takes more than 3
seconds to load. Mobile users are even _less_ patient.

To solve this problem and help provide lots of other really great features, we
developed a technique called **_AJAX_**. As mentioned at the beginning of this
module, AJAX is short for _Asynchronous JavaScript and XML_ (although we'll be
using JSON instead of XML).

In AJAX we:

1. Deliver an initial, engaging page using HTML and CSS which browsers render
   _quickly_
2. _Then_ we use JavaScript to add more to the DOM, behind the scenes

Current implementations of AJAX rely the following  technologies:

- `fetch`
- `Promises`
- `JSON`
- [asynchronous Input / Output][asyncio]
- [the event loop][el]

In this reading and in the following readings and labs, we'll be learning about
all of these topics and how we can implement them in our code to make dynamic,
efficient, and user-friendly websites.

## Explain How to Fetch Data with `fetch()`

The `fetch` function retrieves data. It's a global _method_ on the `window`
object, provided to JavaScript as a browser web API. That means you can use it
simply by calling `fetch()` anywhere in your code and passing in a path to a
resource as an argument.

To use the data that is returned by the `fetch()`, we need to chain on the
`.then()` method. We can see what this looks like below:

```js
fetch("string representing a URL to a data source")
  .then(function (response) {
    return response.json();
  })
  .then(function (data) {
    // Use the data from the response to do DOM manipulation
  });
```

When we write fetch requests in our JavaScript code, this is the form it will
often take - one call to our `fetch` method, followed by two `.then` statements.
Let's walk through each of these pieces of code step by step!

## Fetch Itself

Let's start off with the `fetch` function itself. What's it doing? How are we
using it?

As we discussed in previous sections, clients request data from servers and
servers send data to clients. This is the _request-response cycle_.

`fetch` is used to initiate that request-response cycle from our client-side
code to our servers. When we enter a Google search, for example, Google's
client-side code initiates a `fetch` request (or something similar) for the data
we're searching for.

We use `fetch` by passing it a URL that points toward the server we want to
communicate with. We'll be mainly passing URLs pointing toward `localhost`
servers run by JSON Server, but we can pass it a URL pointing toward any API we
want to work with - this [Game of Thrones](https://anapioficeandfire.com/) API,
for example! When developing the Frontend for a Full Stack application, you'll
pass `fetch` the URL to your backend API.

We'll be using this API to play around with `fetch` during the rest of this
lesson. Add the following code snippet to your `index.js` file! Go ahead and
open your `index.html` file in your browser as well - we won't see any output in
our browser console yet, but we will be using it in later examples.

```JavaScript
fetch("https://anapioficeandfire.com/api/books")
```

Congratulations! You've officially run your first fetch request!

### Fetch is Asynchronous

We discussed JavaScript's asynchronous abilities in some of our previous
lessons. As a brief refresher, _asynchronous_ code is code that allows us to
start an operation in our code, then switch to running other pieces of code
while we wait for that operation to finish.

`fetch` is a prime example of asynchronous code in JavaScript! It also
demonstrates how useful asynchronous programming is.

Think about making a web request - your computer is asking some remote server
for some information. You might be thousands of miles away from the server!

That server may then have to interact with a database to get the information
you've requested. Once the database has finished looking for the appropriate
data and giving it to the server, the server then has to send that data all the
way back to you.

That's a lot to do, and it takes a long time!

If our code had to wait for the server to respond with the data we're asking for
before moving on to other tasks, it could really slow down the performance of
our website. Code that halts other code from running is often called _blocking_
code - it blocks our code's ability to move on to other tasks.

Fortunately, because fetch uses asynchronous JavaScript, it is considered
_non-blocking_ - our code is free to move on to other tasks before our server
gives us a response!

For example, in the following code snippet, our `console.log` will run before we
receive a response back from the server (we'll see this in greater detail a
little further down).

```JavaScript
fetch("https://anapioficeandfire.com/api/books")

console.log("I don't wait!") // runs before the server has returned a response.
```

## Our First .then

Ok, so we can keep doing stuff while waiting for our server to respond - neat.

But what about code that we want to run _after_ our server responds? What about
code that's supposed to actually _use_ that data? That's where our `.then`
statements come in.

### Promises

We're going to talk about this in more detail in a later section, but in order
to understand how `fetch` and `.then` statements work together, we need to talk
a little bit about _Promises_.

A _Promise_ is a piece of data that JavaScript generates when it kicks off an
asynchronous task. It's used to determine whether that asynchronous operation is
still running, if it has completed, or if it failed.

Try updating the code in your `index.js` file to the following code snippet.
Then open your `index.html` file in your browser:

```JavaScript
const myPromise = fetch("https://anapioficeandfire.com/api/books")

console.log(myPromise)
```

If you open up your browser console, you should see the following:
`Promise {<pending>}`.

Because our `fetch` request takes a while to complete, and because we want to
move on to other tasks, it generates this `Promise` object for us, which we can
use to determine when the fetch request finishes.

When Promises are first generated, they have a status of _pending_. That status
can either resolve to _fulfilled_, if the operation is successful, or
_rejected_, if the operation fails.

Once our fetch is complete, the status of this Promise object will change to
_fufilled_.

Let's make the following modification to our code:

```JavaScript
const myPromise = fetch("https://anapioficeandfire.com/api/books")

console.log(myPromise)

setTimeout(() =>{
    console.log(myPromise)
}, 1000)
```

We should now see a new, additional log in our browser console: `Promise
{<fulfilled>: Response}`. (If you don't see it, try increasing the time interval
in setTimeout.)

### Waiting for a Fulfilled Promise

Our `.then` statements are designed to wait for the Promise generated by `fetch`
to be fulfilled. In other words, it waits until we've received the response back
from our server.

In order to give each `.then` statement some code to run once the Promise has
been fulfilled, we need to pass each `.then` statement a _callback function_.

In the case of our `.fetch`, this callback function will run once our server has
sent back the response. It will also be _passed_ the response from the server as
an argument:

```JavaScript
fetch("https://anapioficeandfire.com/api/books")
.then(response => {
  console.log(response)
})
```

If you add this code to your `index.js` file, you should see a new `Response`
object logged to your browser console (you can comment out the previous code to
if you'd like).

If you expand this Response object in the browser console, you should see that
it contains several key/value pairs in it, including `type`, `body`, `status`,
and `url`, among others.

Great! We've successfully made a request to our API and received a response back
from our server! Now it's time to extract the data we want from our response.

### Parsing JSON

When working with the response, we want to extract the data contained within the
response's `body`. The `body` contains all the data we want from our server in
the form of JSON.

We want to convert that data from JSON into actual JavaScript data structures -
arrays, objects, and so on.

If you remember from previous lessons, JSON is technically just a string.
Strings are great for sending data between clients and servers, but they are
hard to work with when we want to interact with data in our JavaScript code.

Fortunately, we can use the built-in `.json` method to convert JSON into the
data structures we want! The `.json` method _parses_ JSON, which means it goes
through the JSON string and converts the symbols in JSON into actual JavaScript
data structures.

Let's include the `.json` method in our code!

```JavaScript
fetch("https://anapioficeandfire.com/api/books")
.then(response => {
    return response.json()
  })
```

## Our Second .then

Just as our completed `.fetch` request will pass the response from our server to
our first `.then` statement, our first `.then` statement will pass our parsed
data to our second `.then` statement. Keep in mind that we _do_ need to
**_return_** our parsed data from the first `.then` statement in order for our
second `.then` statement to receive it.

```JavaScript
fetch("https://anapioficeandfire.com/api/books")
.then(response => {
  return response.json()
})
.then(data =>{
  console.log(data)
})
```

If you update the code in your `index.js` file, you should see the data we
requested from our API being logged to our browser console!

We now have access to the data we want to use from within this second `.then`
statement. We can pass it on to other functions that can then render that
information to our webpage, or complete some other operation using our data:

```JavaScript
const renderData = (data) =>{
  // perform DOM manipulation to display our fetched data
  // Feel free to write more code here!
}

fetch("https://anapioficeandfire.com/api/books")
.then(response => {
  return response.json()
})
.then(data =>{
  renderData(data)
})
```

## GET Requests

The type of `fetch` requests we've covered in this lesson has been running a
`GET` request to our backend server.

As discussed in previous lessons, an HTTP `GET` request is just a simple request
for data from our Frontend to our Backend .

It doesn't require us to send any data from our Frontend to our Backend, nor do
we need to include any specification of the type of HTTP request we're making -
`fetch` makes a `GET` request by default when we pass it a URL as it's only
argument!

We'll learn about sending other types of HTTP requests using `fetch` in upcoming
lessons.

## Working Around Backwards Compatibility Issues

As you can see, `fetch()` provides us with a quick and easy way to request data
from our server within our client-side code.

In older code you might see `jquery.ajax` or `$.ajax` or an object called an
`XMLHttpRequestObject`. You might also see libraries like `axios` used in newer
code.

These are all different tools that accomplish similar tasks as `fetch`. We'll be
using `fetch` throughout the duration of the program, but you can and should
research these older tools if you're interested!

## Identify Examples of the AJAX Technique on Popular Websites

The AJAX technique opens up a lot of uses!

- It allows us to pull in dynamic content. The browser can render the same HTML
  page for a cooking website and update the recipe on display _without_
  reloading the page. This approach was pioneered by GMail whose nav area is
  swapped for mail content swiftly — thanks to AJAX.
- It allows us to get data from multiple sources. We could make a website that
  displays the current weather forecast and the current price of bitcoin side by
  side! This approach is used by most sites to render ads. Your content loads
  while JavaScript gets the ad to show and injects it into your page (sometimes
  AJAX can be used in a way that we don't _entirely_ like).

## Conclusion

Many pages use AJAX to provide users fast and engaging sites. It's not always
necessary - if you're building a static website, basic HTML could suffice.
However, for any site that needs to request data from a server, the AJAX
technique is a great tool to have.

Using `fetch()`, we can include requests for data wherever we need to in our
code. We can `fetch()` data on the click of a button,the expansion of an
accordion display, or the submission of a form. There are many older methods for
fetching data, but `fetch()` is the best option for new code.

## Resources

- [MDN Fetch API](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API)

[sf]: https://en.wikipedia.org/wiki/Serialization
[asyncio]:
  https://developer.mozilla.org/en-US/docs/Learn/JavaScript/Asynchronous/Introducing
[el]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/EventLoop
