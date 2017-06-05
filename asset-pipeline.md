# Rails Asset Pipeline

## What is the asset pipeline?

The asset pipeline is a feature in rails to manage our assets. These include things like...

  * stylesheets (CSS)
  * javascript (JS)
  * images, audio, video, or other media

It is implemented using a gem called `sprockets` that is built into rails.

You might have used it (if you put stuff in ```app/assets```).

### Exercise on Assets

Take 15 minutes to do the following...

1. Copy an image file into your app/assets/images folder. Reference it in your views appropriately (using `image_tag`), and make sure you can see those images in development and production.

2. Add an additional stylesheet to your app (any name should work). Add at least one obvious style. Verify that it works in development and production.

3. Using the web inspector, compare how assets are served in development and production.

### What does it do?

Formerly in Rails (as in Sinatra), you simply served up images, css and javascript as flat files in the public directory.

The asset pipeline can be configured to do any of the following:

  * concatenate CSS and JS
  * compress/minify CSS and JS
  * fingerprint file-names for caching purposes
  * pre-process assets (so they can include ERB, or even SASS / CoffeeScript)
  * provide helper methods to reference assets

### Why?

* **Concatenating** - By serving one file rather than many, the load time of pages can be greatly reduced because the browser makes fewer requests.
* **Compression/minification** - reduces the file size enabling the browser to download it faster.
* **Fingerprinting** file-names allows browsers to cache (avoid downloading the same file twice)
* **SASS** can clean up and improve our CSS
* **Helpers** make it easy to reference our assets in ruby, even when fingerprinting means we don't know the exact file name
