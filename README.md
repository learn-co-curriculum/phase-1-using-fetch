# Use `fetch()`

## Learning Goals

- Explain how to fetch data with `fetch()`
- Working around backwards compatibility issues
- Identify examples of the AJAX technique on popular websites

## Introduction

When it comes to making engaging web sites, we often find ourselves needing to
send a lot of data (text, images, media, etc.). But sending a **huge** amount
of data when you first land on a page puts a huge burden on the user's
computer. The user's computer has to build the DOM and integrate all those
pieces, retrieving and stitching them together. Users experience this process as
slowness and too much of it means they'll click away and never come back.

Web users expect sites to load quickly **and** to stay updated. Research shows
that 40 percent of visitors to a website will leave if the site takes more than
3 seconds to load. Mobile users are even _less_ patient.

We deliver sites that keep users engaged by using a technique called
***AJAX***. In AJAX we:

1. Deliver an initial, engaging page using HTML and CSS
2. Use JavaScript to add more to the DOM, behind the scenes

AJAX relies on several technologies:

* Things called `Promise`s
* Things called `XMLHttpRequestObject`s
* A [serialization format][sf] called JSON
* [asynchronous Input / Output][asyncIO]
* [the event loop][el]

We're going to gloss over all these pieces in this lesson. By doing so, we can
_see_ how the JavaScript `fetch()` function works, and experience the AJAX
technique.

> **STRETCH**: If you were to apply for a software developer position, you
> would be expected to understand each of the "glossed over" elements. This
> course will provide additional material to introduce you to the key
> technology. If, on the other hand, you're not seeking that deeper technical
> understanding, this introduction should be enough.

Let's learn to use `fetch()` to apply the AJAX technique: a way to load
additional data _after_ information is presented to the user.

## Explain How to Fetch Data with `fetch()`

The `fetch()` function retrieves data. It's a global _method_ on the `window`
object. That means you can simply use it with `fetch()`.

Here's the skeleton for using it:

```js
fetch("string representing a URL to a data source")
  .then(response => response.json())
  .then(json => ...)
```

The first thing you need is a `String` that represents a URL that can provide
you some data. As it happens, http://api.open-notify.org/astros.json will
provide a list of the humans in space. You can paste this URL into a browser
tab and see that this URL returns a JSON structure. You provide this string as
the first argument to `fetch()`.

You will want to _chain_ a call to `then()` at the end of `fetch()`.

> **REMEMBER**: Since JavaScript doesn't care about whitespace
> 
> ```js
> fetch("string representing a URL to a data source").then(response => response.json());
> ```
> 
> is the same as:
> 
> ```js
> fetch("string representing a URL to a data source")
>   .then(response => response.json());
> ```

The `then()` takes a function. Here is where you tell JavaScript to ask the
network response to be turned into JSON.  When starting out, this first
`then()` will pretty much be the same across all your uses of `fetch()`.

The final `then()` is when you actually get some JSON passed in. You can then
do something with the JSON. The easiest thing is to `console.log()` the JSON
_or_ to hand the JSON off to another function.

```js
fetch('http://api.open-notify.org/astros.json')
  .then(response => response.json())
  .then(json => console.log(json));
```

![kimmy wow](http://i.giphy.com/3osxYwZm9WZwnt1Zja.gif)

Let's perform a trivial demonstration. Open up a new **incognito** tab in
Chrome. Open up DevTools and paste the following:

```js
fetch('http://api.open-notify.org/astros.json').then(response => response.json()).then(json => document.write(`Holy cow! There are ${json["number"]} humans in space.`));
```

![Simple fetch()](https://curriculum-content.s3.amazonaws.com/skills-front-end-web-development/js-async-fetch-readme/simple_fetch_incog_window.png)

You might notice that this chained method call returned a `Promise` in the
DevTools console. We'll cover that later.

## Working Around Backwards Compatibility Issues

As you can see, `fetch()` provides us with a short way to fetch and work with
resources. However, `fetch()` has only recently arrived in browsers. In older
code you might see `jquery.ajax` or `$.ajax` or an object called an
`XMLHttpRequestObject`.

## Identify Examples of the AJAX Technique on Popular Websites

The AJAX technique opens up a lot of uses!

* It allows us to pull in dynamic content. The same "framing" HTML page remains
  on screen for a cooking website. The recipe on display updates _without_ page
  load. This approach was pioneered by GMail whose nav area is swapped
  for mail content swiftly &mdash; thanks to AJAX.
* It allows us to get data from multiple sources. We could make a website that
  displays the current weather forecast and the current price of bitcoin side
  by side! This approach is used by most sites to render ads. Your content loads
  while JavaScript gets the ad to show and injects it into your page (sometimes
  AJAX can be used in a way that we don't _entirely_ like).

## Conclusion

Many pages use AJAX to provide users fast and engaging sites. It's certainly
not required in all sites. Using it for every site is a step backward when
simple HTML would suffice. However, as sites have more and more material, the
AJAX technique is a great tool to have.

Using `fetch()`, we can include requests for data wherever we need to in
our code. We can `fetch()` data on the click of a button or the expansion of an
accordion display. There are many older methods for fetching data, but
`fetch()` is the future.

## Resources

- [MDN Fetch API](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API)

<p class='util--hide'>View <a href='https://learn.co/lessons/fewpjs-use-fetch'>Use Fetch</a> on Learn.co and start learning to code for free.</p>

[sf]: https://en.wikipedia.org/wiki/Serialization
[asyncIO]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/EventLoop
[el]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/EventLoop
