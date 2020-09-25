# Lokl.dev website source code

Lokl is a WordPress local development environment for Mac, Win & Linux.

This repository holds the [Lokl website](https://lokl.dev) content

## Tech used

 - [Hugo](https://gohugo.io) static site generator does the heavy lifting
 - [Accessible Minimalism hugo theme](https://github.com/leonstafford/accessible-minimalism-hugo-theme) keeps it fast and friendly

TODO:

 - [HTML Tidy](https://github.com/htacg/tidy-html5) to beautify things

And, probably, some shell scripts, if not already then eventually!

The site is currently served from CloudFlare, using their Workers Sites. A
 GitHub Action checks out the code, builds the Hugo site and then uses Wrangler
 to deploy the site content to CloudFlare (using their KV store behind the
 scenes).

## Testing

 - [Validates to XHTML 1.0 Strict](https://validator.w3.org/check?uri=https%3A%2F%2Flokl.dev)
 - [Avoids a11y errors](https://wave.webaim.org/report#/https://lokl.dev/)
