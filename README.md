# Mobile Web Specialist Certification Course

---

#### _Three Stage Course Material Project - Restaurant Reviews_

## Project Overview: Stage 1

For the **Restaurant Reviews** projects, you will incrementally convert a static webpage to a mobile-ready web application. In **Stage One**, you will take a static design that lacks accessibility and convert the design to be responsive on different sized displays and accessible for screen reader use. You will also add a service worker to begin the process of creating a seamless offline experience for your users.

### Specification

You have been provided the code for a restaurant reviews website. The code has a lot of issues. It’s barely usable on a desktop browser, much less a mobile device. It also doesn’t include any standard accessibility features, and it doesn’t work offline at all. Your job is to update the code to resolve these issues while still maintaining the included functionality.

### Project Rubric

Your project will be evaluated by a Udacity code reviewer according to the [Restaurant Reviews project rubric](https://review.udacity.com/#!/rubrics/1090/view). Please review for detailed project requirements. The rubric should be a resource you refer to periodically to make sure your project meets specifications.

### What do I do from here?

1. In this folder, start up a simple HTTP server to serve up the site files on your local computer. Python has some simple tools to do this, and you don't even need to know Python. For most people, it's already installed on your computer.

   - In a terminal, check the version of Python you have: `python -V`. If you have Python 2.x, spin up the server with `python -m SimpleHTTPServer 8000` (or some other port, if port 8000 is already in use.) For Python 3.x, you can use `python3 -m http.server 8000`. If you don't have Python installed, navigate to Python's [website](https://www.python.org/) to download and install the software.
   - Note - For Windows systems, Python 3.x is installed as `python` by default. To start a Python 3.x server, you can simply enter `python -m http.server 8000`.

2. With your server running, visit the site: `http://localhost:8000`, and look around for a bit to see what the current experience looks like.
3. Explore the provided code, and start making a plan to implement the required features in three areas: responsive design, accessibility and offline use.
4. Write code to implement the updates to get this site on its way to being a mobile-ready website.

## Leaflet.js and Mapbox:

This repository uses [leafletjs](https://leafletjs.com/) with [Mapbox](https://www.mapbox.com/). You need to replace `<your MAPBOX API KEY HERE>` with a token from [Mapbox](https://www.mapbox.com/). Mapbox is free to use, and does not require any payment information.

### Note about ES6

Most of the code in this project has been written to the ES6 JavaScript specification for compatibility with modern web browsers and future-proofing JavaScript code. As much as possible, try to maintain use of ES6 in any additional JavaScript you write.

## Code

After Few days of trouble with the server I made this app Responsive on any device, Iphone, Ipad,Desktop.
I started with the mobile version and I worked all the way up to the desktop version min-width 1000px.

in the 2 HTML pages available I added on links and inputs, accessibility Aria language for screen reader.

I used tabindex in order to jump from a link to another and I set some label and role on the various tags.

After the initial stage I added the service worker to the app, caching all the data to make the appa available even offline.

The trick with the service Worker is to put the sw.js file where the html files are and not with the others js files, otherwise does not work at all.

I added the registration in the index.html file, and the rest in the sw.js file.

Below the code that I used in sw.js

```javascript
const cacheFiles = [
  "/",
  "./index.html",
  "./restaurant.html",
  "./css/styles.css",
  "./js/dbhelper.js",
  "./js/main.js",
  "./js/restaurant_info.js",
  "./data/restaurants.json",
  "./img/1.jpg",
  "./img/2.jpg",
  "./img/3.jpg",
  "./img/4.jpg",
  "./img/5.jpg",
  "./img/6.jpg",
  "./img/7.jpg",
  "./img/8.jpg",
  "./img/9.jpg",
  "./img/10.jpg"
];

self.addEventListener("install", function(e) {
  e.waitUntil(
    caches.open("t1").then(function(cache) {
      return cache.addAll(cacheFiles);
    })
  );
});

self.addEventListener("fetch", function(e) {
  e.respondWith(
    caches.match(e.request).then(function(response) {
      if (response) {
        console.log("Found ", e.request, " in cache");
        return response;
      } else {
        console.log("Could not find ", e.request, " in cache, Fetching!");
        return fetch(e.request)
          .then(function(response) {
            const clonedResponse = response.clone();
            caches.open("t1").then(function(cache) {
              cache.put(e.request, clonedResponse);
            });
            return response;
          })
          .catch(function(err) {
            console.error(err);
          });
      }
    })
  );
});
```

Very important is to check all the files that you cache, because even if 1 of them is wrong (with wrong I mean there is a typo error), the fetch will give you a promise error.
