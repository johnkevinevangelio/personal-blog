---
layout: post
title:  "How Browsers render HTML"
date:   2020-09-28 02:07:20 +0800
---

### Browser Components

![Browser Components](/personal-blog/assets/blog3/browser_components.png)
##### Image Source: *<https://www.html5rocks.com>*

### Browser Engine

A **browser engine** is the portion of a web browser that works "under the hood" to fetch a web page from the internet, and translate its contents into forms you can read, watch, hear, etc.

**Popular Browser Engines**

Engine | Status | Steward | License | Embedded in
--- | --- | ---
WebKit | Active | Apple | GNU LGPL, BSD-style | Safari, all browsers hosted on the iOS App Store
Blink | Active | Google | GNU LGPL, BSD-style | Google Chrome and all other Chromium-based browsers such as Microsoft Edge, Brave and Opera
Gecko | Active | Mozilla | Mozilla Public | Firefox browser and Thunderbird email client plus forks such as Sea Monkey and Waterfox
Trident | Discontinued | Microsoft | Proprietary | Internet Explorer browser and Microsoft Outlook email client

The User interface we see in our browsers such as tabs, toolbar, menu, etc are called **Chrome**

#### Sub-components

- **HTTP Client or Network Engine**
- **Layout Engine or Rendering Engine** (includes HTML parser and CSS parser)
- **Javascript Engine** (also composed of parsers, interpreters, and compilers of its own)

### How the browser renders a web page?

#### First:

A browser sends a request to the server to fetch an HTML document, the server returns an HTML page in binary stream format which is basically a text file with a response header Content-Type which has value text/html; charset=UTF-8. Here text/html is a MIME Type which tells the browser that it is an HTML document and charset=UTF-8 tells the browser that it is encoded in UTF-8 character encoding. Using this information, the browser can convert the binary format into a readable text file.

#### Second:

Using a HTML parser(using any programming language), **create a Document Object Model (DOM) tree**. DOM is tree of nodes. A node is an object (HTMLTitleElement: { tag_name: String, attributes: hashmap } ). 

![DOM](/personal-blog/assets/blog3/DOM.png)
##### Image Source: *<https://medium.com/>*

#### Third:

Using a CSS parser, **create CSSOM tree**.

![DOM](/personal-blog/assets/blog3/CSSOM.png)
##### Image Source: *<https://medium.com/>*

#### Fourth:

**Create Render-Tree**. Render-Tree is a tree-like structure constructed by combining DOM and CSSOM trees. The browser has to calculate the layout of each visible element and paint them on the screen, for that browser uses Render-Tree. Hence, unless Render-Tree isn’t constructed, nothing will get printed on the screen.

#### Fifth: 

**Painting**. Since elements (or a sub-tree) in the Render-Tree can overlap each other and they can have properties that make them frequently change the look, position or geometry, the browser creates a layer for it. 

Now that we have layers, we can combine them and draw it on the screen. But the browser does not draw all the layers in a single image. Each layer is drawn separately first.
Inside each layer, the browser fills the individual pixels with whatever visible property of the element is like border, background colour, shadow, text, etc. This process is also called as rasterization. To increase performance, the browser may use different threads to perform rasterization.

#### Sixth:

**Parse Javascript** using javascript compiler.


#### List of Rendering Engines written in different Programming Language

- CSSBox (Java)
- Cocktail (Haxe)
- gngr (Java)
- litehtml (C++)
- LURE (Lua)
- NetSurf (C)
- Servo (Rust)
- Simple San Simon (Haskell)
- WeasyPrint (Python)
- WebWhirr (C++)

### Sources:

1. *[Matt Brubeck](https://limpet.net/mbrubeck/2014/08/08/toy-layout-engine-1.html)*
2.  *[html5rocks.com](https://www.html5rocks.com/en/tutorials/internals/howbrowserswork/)*
3.  *[JSConf](https://www.youtube.com/watch?v=0IsQqJ7pwhw&list=PLZqtz6Oug4fRj0fRv89caQZQCdwDhMcH3&index=4&ab_channel=JSConf)*
4.  *[JsPoint](https://medium.com/jspoint/how-the-browser-renders-a-web-page-dom-cssom-and-rendering-df10531c9969#:~:text=When%20a%20web%20page%20is,the%20Render%2DTree%20from%20it.)*
5.  *[Wikipedia](https://en.wikipedia.org/wiki/Comparison_of_browser_engines)*