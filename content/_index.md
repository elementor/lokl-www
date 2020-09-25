---
---

## WordPress local development environment for Mac, Win & Linux

*Secure. Fast. Free!*

### Getting started with Lokl

 - Ensure Docker and cURL are installed
 - Copy and paste this one line to your terminal:

`sh <(curl -sSl https://lokl.dev/go?v=1)`

 - Follow the simple wizard to create a new site or manage existing ones
 - Publish your live site using built-in static site generator plugins & tools

#### Demo

[preview of Lokl without captions or audio description](https://youtu.be/MuWIXLUQOF0)

#### How it works

Lokl relies on the free Docker virtualization in order to create a small,
 containerized environment for each Lokl WordPress site you want to run. 

The Lokl project contains several pieces that work together to provide a very
 easy way to create and maintain 100's or 1,000's of WordPress environments on
 your local computer:

**Lokl Docker image**

You may consider this to be a pre-built mini operating system, with WordPress
 built-in and a bunch of optimizations added.

**Lokl Docker provisioning scripts**

These take the pre-built Lokl Docker image and apply site-specific
 customizations, so that each site is independent and easily manageable via its
 unique name.

**Lokl shell scripts**

This is the entrypoint and main control point for Lokl. It's the POSIX-compliant
 shell script available at `https://lokl.dev/go` or downloadable to run as a
 local script. This script provides you with an easy to use CLI (command line
 interface) wizard, that responds to your input and interacts with your
 WordPress container(s) in the background.


#### Requirements

**[Docker](https://www.docker.com/)**

As I describe how Lokl works above, Docker is a base requirement for Lokl to
 run. It is free to install on Lokl's supported platforms:

 - [Docker installation for macOS](https://docs.docker.com/docker-for-mac/install/)
 - [Docker installation for Linux](https://docs.docker.com/engine/install/)
 - [Docker installation for Windows](https://docs.docker.com/docker-for-windows/install/)

*Please note, that Lokl currently expects you to have Docker setup to accept
 commands as the current user, not with `sudo` privilege escalation, though I'll
 try to get that more seamlessly supported in future.*

**[cURL](https://curl.haxx.se/)**

I use this in the one-line launch wizard on the homepage, but also within
 Lokl's behind the scenes scripts. It's free, available by default in macOS and
 easily installable on all other environments.

It's worth mentioning that cURL has been generously developed for years by
 [Daniel Stenberg](https://daniel.haxx.se), along with many contributors. You
 may not be aware, but you'll have run some software or websites in your life
 that benefit from cURL. Thanks Daniel!

If you need help installing cURL for your environment, please ask on my
 [support forum](https://staticword.press).

**supported shell environment**

The main Lokl shell script is designed to be POSIX-compliant and compatible with
 the most common shells/terminals you are likely to be running. If you encounter
 an issue running Lokl in your funky shell, please [file an issue](https://github.com/lokl-dev/go).


##### FAQs

 - can I use this in production?

*I'd strongly dissuade you! Lokl is designed for your local WordPress development
 environment and expects you to publish your live site using a static site
 generator for WordPress (I include the most common ones within Lokl for you).

Running WordPress locally and publishing a static site to production hits the
 three goals of security, cost and performance excellently. Lokl is designed for
 running WordPress on your own computer, comfortably isolated within its own
 container. This means I don't need to waste time on the usual security
 measures one would want to implement and maintain on a publicly exposed
 WordPress site.*

 - does Lokl work on the BSDs?

*No, as Docker won't run on them. If you have some cool hack to allow Docker to
 run on any of the BSDs, then it should work and I'd love to hear from you!*

 - why aren't Lokl sites served with TLS over https?

*See the FAQ about why Lokl isn't for running in production. TLS isn't required
 for such locally contained web servers and would limit the portability and ease
 of use of Lokl if I was to bother with it.*

 - can I use custom hostnames for each of my Lokl sites?

*Why do you need this? Lokl comes with built-in, name-based management of all
 your sites, which will be in the form of `http://localhost:4123`, with a
 unique port within the range of 4000-5000. This allows the super-simple
 one-line installation/management script to work without you needing to
 configure anything on your host operating system.

If you really needed to map a custom hostname locally to one of your Lokl sites,
 you can definitely do it yourself, via some creative proxying, but it's not
 something I'm likely to want to help you with unless it would make sense for a
 lot of Lokl users.*
