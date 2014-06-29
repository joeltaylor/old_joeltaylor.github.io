---
layout: post
title: "Mobile caching"
date: 2014-06-29 14:09:26 -0400
comments: true
categories: Javascript Mobile
---
<p>A client reported a <em>bug</em> when using their iPad to navigate their site.
When the user selects a page from the dropdown mobile navigation and then presses
"Back" they return to the previous page with the navigation menu still open. I tried
to replicate the issue on my iPhone (4s) with no luck but when I ran the iPad simulator
in Xcode I was able to replicate it.<p>

<p>I never experienced this sort of behavior before but then again, when testing sites,
I don't always navigate backwards. My first attempt was to hide the navigation on page
load but the event wouldn't trigger on the iPad.</p>

<p>It turns out that many mobile devices use
<a href="https://developer.mozilla.org/en-US/docs/Using_Firefox_1.5_caching">back-forward cache
</a>to decrease load time. In order to cause the page to reload as opposed to serving the
cached page you can use this javascript : </p>

``` javascript
  window.onpageshow = function(event) {
    if (event.persisted) {
      document.body.style.display = "none"; //remove the occasional page flash
      location.reload();
    }
};
```

<p>Of course, this method is a bit backwards as I don't really need to reload the
entire page. Instead, I can just resort back to hiding the navigation to take
advantage of the page caching.</p>

