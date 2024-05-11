---
layout: post
title: "Using tailwind css with hyperscript"
date: 2024-05-11 11:22:48 +0200
categories: software
tags: software htmx tailwind css hyperscript
author: developer
---

## Using tailwind css with hyperscript

While my development journey has primarily revolved around htmx rather than the more conventional choice of
React, I've realized that even with htmx, there are instances where introducing client-side interactivity is
essential for enhancing the user experience and functionality of web applications.

Tailwind CSS simplifies styling by providing a vast collection of utility classes that developers can apply
directly to HTML elements. This utility-first approach allows for rapid prototyping and customization,
enabling developers to create visually appealing interfaces with ease.

In my experience integrating Tailwind CSS with React, you use react state managment, like useState to manage
the component interactivity and styles changes. However, when working without React, managing state changes,
such as adjusting styles for a toggle component styled with Tailwind, requires additional JavaScript to ensure
smooth interactivity.

```html
<!-- Enabled: "bg-indigo-600", Not Enabled: "bg-gray-200" -->
<button
  type="button"
  class="bg-gray-200 relative inline-flex h-6 w-11 flex-shrink-0 cursor-pointer rounded-full border-2 border-transparent transition-colors duration-200 ease-in-out focus:outline-none focus:ring-2 focus:ring-indigo-600 focus:ring-offset-2"
  role="switch"
  aria-checked="false"
>
  <span class="sr-only">Use setting</span>
  <!-- Enabled: "translate-x-5", Not Enabled: "translate-x-0" -->
  <span
    aria-hidden="true"
    class="translate-x-0 pointer-events-none inline-block h-5 w-5 transform rounded-full bg-white shadow ring-0 transition duration-200 ease-in-out"
  ></span>
</button>
```

If you try to use this code directly you are going to end up with a static toggle that don't change:

![Tailwind toggle]({{ "/assets/images/toggle.png?style=centerimage" | relative_url }})

This is where Hyperscript becomes useful, allowing for easy control of HTML elements and state changes, making
it a great match for managing Tailwind CSS styles in non-React setups.

From hyperscrip.org documentation site:

> Hyperscript is a scripting language for doing front end web development. It is designed to make it very easy
> to respond to events and do simple DOM manipulation in code that is directly embedded on elements on a web
> page.

So if you need to change the components styles when clicking the toggle button you could do something like
this:

```html
<button
  type="button"
  _="on click
        toggle between .translate-x-0 and .translate-x-5 on <span:nth-child(2)/> 
        toggle between .bg-indigo-600 and .bg-gray-200 on me
    end"
  class="bg-gray-200 relative inline-flex h-6 w-11 flex-shrink-0 cursor-pointer rounded-full border-2 border-transparent transition-colors duration-200 ease-in-out focus:outline-none focus:ring-2 focus:ring-indigo-600 focus:ring-offset-2"
  role="switch"
  aria-checked="false"
>
  <span class="sr-only">Use setting</span>
  <!-- Enabled: "translate-x-5", Not Enabled: "translate-x-0" -->
  <span
    aria-hidden="true"
    class="translate-x-0 pointer-events-none inline-block h-5 w-5 transform rounded-full bg-white shadow ring-0 transition duration-200 ease-in-out"
  ></span>
</button>
```

What's happening here:

**#1** `on click ... end`: This part of the attribute defines an event handler for the "click" event on the
toggle button.

**#2** `toggle between ... and ... on ...`: These are directives within the event handler. They specify
actions to be taken when the button is clicked.

**#3** `toggle between .translate-x-0 and .translate-x-5 on <span:nth-child(2)/>`: This directive toggles the
classes .translate-x-0 and .translate-x-5 on the second child `<span>` element when the button is clicked.

**#4** `toggle between .bg-indigo-600 and .bg-gray-200 on me`: This directive toggles the background color
between .bg-indigo-600 and .bg-gray-200 on the button itself when clicked.

You can read more about [hyperscript events](https://hyperscript.org/features/on/). Here is the final output:

![Tailwind css with hyperscript component]({{ "/assets/images/toggle.gif?style=centerimage" | relative_url }})

And here is the complete example

```html
<!DOCTYPE html>
<head>
    <title>Toggle using Tailwind css with Hyperscript</title>
    <meta name="viewport" content="width=device-width,initial-scale=1">
    <script src="https://cdn.tailwindcss.com"></script>

</head>
<body>
    <div class="h-screen flex items-center justify-center">
        <button
            type="button"
            _="on click
                toggle between .translate-x-0 and .translate-x-5 on <span:nth-child(2)/>
                toggle between .bg-indigo-600 and .bg-gray-200 on me
            end"
            class="bg-gray-200 relative inline-flex h-6 w-11 flex-shrink-0 cursor-pointer rounded-full border-2 border-transparent transition-colors duration-200 ease-in-out focus:outline-none focus:ring-2 focus:ring-indigo-600 focus:ring-offset-2"
            role="switch"
            aria-checked="false"
        >
            <span class="sr-only">Use setting</span>
            <!-- Enabled: "translate-x-5", Not Enabled: "translate-x-0" -->
            <span
            aria-hidden="true"
            class="translate-x-0 pointer-events-none inline-block h-5 w-5 transform rounded-full bg-white shadow ring-0 transition duration-200 ease-in-out"
            ></span>
        </button>
    </div>
    <script src="https://unpkg.com/hyperscript.org@0.9.12"></script>
</body>
</html>
```
