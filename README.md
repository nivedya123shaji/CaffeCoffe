# Coffee-shop using vanilla javascript

Build Coffee shop website using vanilla javascript

## DOM

**DOM**: The Document Object Model connects web pages to JavaScript by representing the structure of a document in memory.

## DOM API

**DOM API**: It's An API exposed by a browser to developers to manipulate the DOM from a Scripting language (Like javascript). So we can change a property, and add an element to that structure in memory. That will trigger an update in the rendering HTML that we see on the screen.

The DOM API is available on many objects: 
- window global object
- document object or dom (represent the current dom because we can have multiple documents in one window).
- One object per HTML element and other nodes in our document. (every HTML element in our web app will have an object, and that object will have a dom API.)

## Things that I learned:

### querySelector()

querySelector: The Document method querySelector() returns the first Element within the document that matches the specified selector, or group of selectors. If no matches are found, null is returned.
if we want to select multi-element we should use querySelectorAll(), 

to optimize app performance in JavaScript, we shouldn't query the whole document for a specific element.
instead, we should query the specific part and then get the element we want, for example: 

```
    let nav = document.querySelector("nav");
    //We use the nav variable for caching the reference
    nav.querySelector("a");

```

> In the code above we query the ```nav``` section then we query the ```a``` which in the nav, so we don't use the document to find ```a``` => we are narrowing the query for being more performant.
because the DOM API  is not just available in the document object, but also available in every DOM element.

### "DOMContentLoaded"

**DOMContentLoaded** is an event that we will need when we use defer or async to make sure that the DOM is ready for manipulation because in some browsers even if the parser has finished but the DOM structure in memory is not finished yet, that's rarely happening but it can happen, so we make sure that DOM is ready or not by ```DOMContentLoaded``` event.
> More about async and defer in the Q And A section.<br/>
> Difference between load event and DOMContentLoaded event also in Q And A section.

### Event binding

The event binding allows you to add an event handler for a specified event so that your chosen JavaScript function will be invoked when that event is triggered for the associated DOM element. This can be used to bind to any event, such as keypress, mouseover, or mouseout.

#### Binding functions to events in DOM objects

Events are signals fired inside the browser window that notify of changes in the browser or operating system environment. Programmers can create event handler code that will run when an event fires, allowing web pages to respond appropriately to change.

To bind functions to events in DOM objects there are two ways:

- onevent properties: (named by prefixing "on" to the name of the event). These properties are called to run associated handler code when the event is fired, and may also be called directly by your own code.

- addEventListener: The addEventListener() method of the EventTarget interface sets up a function that will be called whenever the specified event is delivered to the target.

> Check Q And A for the difference between the Two.

### Dispatching Custom Events

We can not only assign handlers but also generate our own events from JavaScript.

Custom events can be used to create “graphical components”. For instance, a root element of our own JS-based menu may trigger events telling what happens with the menu: open (menu open), select (an item is selected), and so on. Another code may listen to the events and observe what’s happening with the menu.

We can generate not only completely new events, that we invent for our own purposes, but also built-in ones, such as click, mouse down, etc. That may be helpful for automated testing.

```

const event = new Event("myCustomName");
element.dispatchEvent(event);

```

> In the code above we dispatch events over an element.

### Routing using History API

The History API provides access to the browser's session history (not to be confused with WebExtensions history) through the history global object. It exposes useful methods and properties that let you navigate back and forth through the user's history, and manipulate the contents of the history stack.

```
// pushing a new URL; the second argument is unused
history.pushState(optional_state, null, "/new-url");
// to listen for changes in URL within the same page navigation
window.addEventListener("popstate", event => {
 let url = location.href;
});

```

>**optional_state**: An optional state object, where we can pass metadata or anything that can be serialized.
>**null**: This parameter exists for historical reasons, and cannot be omitted; passing an empty string is safe against future changes to the method.
>**new-url**: The new history entry's URL. Its client side only url, that's why we call it "fake url"!.

> [!WARNING]
> **Popstate** won't be fired if the user clicks on an external link or changes the URL manually. because history api work using history of our app.

### Web Component

Web components are a set of browser APIs that you can use to create your own custom HTML elements.
It's actually a set of standards:
  - Custom Elements
  - HTML Templates
  - Shadow DOM
  - Declarative Shadow DOM (new)

  - **Custom Elelments**

  A way to define new, reusable HTML elements with custom behavior and functionality using JavaScript.
  ex: (page 114 in pdf slides).

  - **Template Element**

  Fragments of markup that can be cloned and inserted into the document at runtime, with reusable HTML content that can be rendered and modified dynamically (using javascript, The parser ignore template because it conseder it as script).
  It's a way that we have to define the HTML of a web component in HTML file from javascript and inject it in DOM.
  ex: (page 121)

  - **Shadow DOM**
  A private, isolated DOM tree within a web component that is separate from the main document's DOM tree.
  ex: (page 126).

  - **Declarative Shadow DOM**
  A way to define Shadow DOM directly in HTML markup using a new set of attributes and tags.
  > [!WARNING]
  > It's not supported by the most browser at the moment

The mix of ```custom element```, ```template``` using template string, and ```shadow DOM```, The three in action what we know as a Web component.

### Reactive Programming with Proxies

**Proxy** : A wrapper object that lets you intercept and modify operations performed on the wrapped object, allowing you to  add custom behavior or validation to the object's properties and methods.

> In simple terms: It's a simple way that we have in Javascript to listen for changes in an object.

For more info have a look to silde 135 in pdf.

## Q And A?

1. Why should you put a script tag in the bottom of the body and why do we use defer or async?

 Notice: using script tag at the bottom of the body now is deprecated,
 but the reason why we put the script tag at the bottom of HTML in the past is that when the browser is parsing the HTML line by line when it finds the script tag it stops parsing 
 and try to execute a javascript file, so the user should wait until the browser executes javascript so he can see some UI, which we put in the bottom to make HTML and CSS 
 load first, then start executing the javascript code.

 now we are using defer or async: the parser when it finds the script tag with defer will download the file but it will continue parsing the HTML file when it parses 
 the whole HTML file then it will try executing our javascript file.

 async is more suitable for small scripts, that need to be executed as soon as possible because async downloads the js file but when the file is ready it will stop 
 parsing and start executing the js file so Asyn doesn't help to parse too much, if you don't really know which one should use, use defer.

2. Difference between the load event and the DOMContentLoaded event?

The load waits for everything on the page to be loaded (videos, fonts, images...) so we miss the opportunity to manipulate the DOM earlier. DOMContentLoaded event is a soon as possible event, if some parts haven't loaded yet we can manipulate the DOM => so the best option is to use DOMContentLoaded.

3. Difference between onevent and addEventListener?

When we use ``` onevent ``` technique, only one function can be attached per event/object combination:

```
    function eventHandler(event) {

    }

    element.onload = eventHandler;

    element.onload = (event) => {
        //It replaces the first handler, and its property so it uses a setter and getter,
        //There is place for only one event handler
    }

```

but for ``` addEventListener ``` it uses the observer design pattern, where I can subscribe a lot of listeners or observers and all of them will be fired, Also ``` addEventListener ``` supports more advanced event handling.

```
 function eventHandler(event) {
        console.log(options.once);
        options.once = false;
        console.log(options.once);
    }
    
    const options = {
        once: true,
        passive: true
    }
    
    element.addEventListener("click", eventHandler, options);

```

> For example in the above code we can pass a third argument with some options, like an option ```once``` I can use it to trigger an event only once so when the event happens I will turn to a false inside event handler.<br/>
> For the passive option, enables developers to opt-in to better scroll performance by eliminating the need for scrolling to block on touch and wheel event listeners<br/>.

**_More About Passive_**

**Problem**: All modern browsers have a threaded scrolling feature to permit scrolling to run smoothly even when expensive JavaScript is running, but this optimization is partially defeated by the need to wait for the results of any ```touchstart``` and ```touchmove``` handlers, which may prevent the scroll entirely by calling preventDefault() on the event.<br/>

**Solution: {passive: true}**

By marking a touch or wheel listener as passive, the developer is promising the handler won't call preventDefault to disable scrolling. This frees the browser up to respond to scrolling immediately without waiting for JavaScript, thus ensuring a reliably smooth scrolling experience for the user.

ex: 

```

document.addEventListener("touchstart", function(e) {
    console.log(e.defaultPrevented);  // will be false
    e.preventDefault();   // does nothing since the listener is passive
    console.log(e.defaultPrevented);  // still false
}, Modernizr.passiveeventlisteners ? {passive: true} : false);

```

> Once and passive are Advanced Event Handling.

4. Why we use type module in our script tag?

```<script src="app.js" defer type="module"></script>```

Adding type module we turn all project in form of modules, Because if We are not using modules
in javscript everything is global, that's can lead to conflicts between variables (global ones).
So by using module each file has its own context.
