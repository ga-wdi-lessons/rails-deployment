# Rails Asset Pipeline

### What is the asset pipeline?

The asset pipeline is a feature in rails to manage our assets:

* stylesheets (CSS)
* javascript (JS)
* images

It is implemented using a gem called 'sprockets' that is built into rails.

You might have used it (if you put stuff in ```app/assets```).

### Exercise on Assets

Take 15 minutes to do the following:

1. Copy an image file into your app/assets/images folder. Reference it in your
views appropriately (using `image_tag`), and make sure you can see those
images in dev and prod.

2. Add an additional stylesheet to your app (any name should work). Add at least
one obvious style. Verify that it works in dev and prod.

3. Using the web inspector, compare how assets are served in dev and prod.

### What does it do?

Formerly in Rails (as in Sinatra), you simply served up images, css and javascript as flat files in the public directory.

The asset pipeline can do any of the following (configurable):

* concatenates CSS and JS
* compresses/minifies CSS and JS
* fingerprints file-names for caching purposes
* pre-processes assets (so they can include ERB, or even SASS / CoffeeScript)
* provides helper methods to reference assets

### Why?

* **Concatenating** - By serving one file rather than many, the load time of pages can be greatly reduced because the browser makes fewer requests.
* **Compression/minification** - reduces the file size enabling the browser to download it faster.
* **Fingerprinting** file-names allows browsers to cache (avoid downloading the same file twice)
* **SASS** can clean up and improve our CSS
* **Helpers** make it easy to reference our assets in ruby, even when fingerprinting means we don't know the exact file name
