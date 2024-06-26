# Nginx-Fancyindex-Theme
A responsive theme for [Nginx](https://www.nginx.org/) Fancyindex module. Minimal, modern and simple.
Comes with a search form, aims to handle thousands of files without any problems.

The fancyindex module can be found [here](https://github.com/aperezdc/ngx-fancyindex) (by @aperezdc).

[![made-for-nginx](https://img.shields.io/badge/Made%20for-nginx-1f425f.svg)](https://www.nginx.org/)

## Changes from [Original Repo](https://github.com/Naereen/Nginx-Fancyindex-Theme)

- Removed jQuery
- Replaced showdown with [micromark](https://github.com/micromark/micromark) (+ [micromark-extension-gfm](https://github.com/micromark/micromark-extension-gfm))
  - Currently imported as ESM via esm.sh
- Removed xregexp-all.js (not sure what this was used for)
- Removed `Nginx-Fancyindex-Theme` folder (not sure what the difference was compared to the light theme)

Removing unnecessary dependencies and using a much smaller Markdown parser reduced the page size by more than 90%.

### Demonstration of the Light theme:
![Demo #5](screenshots/Nginx-Fancyindex-Theme__example5.png "Example of Nginx-Fancyindex-Theme-light")

## Usage

1. Make sure you have the fancyindex module compiled with nginx, either by compiling it yourself or installing nginx via the full distribution (paquet `nginx-extras`).
2. Include the content of [fancyindex.conf](fancyindex.conf) in your location directive (`location / {.....}`) in your nginx config (usually `nginx.conf`).
3. Move the `Nginx-Fancyindex-Theme-light/` *and/or* `Nginx-Fancyindex-Theme-dark/` folder to the root of the site directory.
   - Alternatively, `header.html` and `footer.html` can be adjusted so that assets are loaded from a different subpath by replacing `/Nginx-Fancyindex-Theme-light/`/`/Nginx-Fancyindex-Theme-dark/`.
4. Restart/reload nginx.
5. Check that it's working, and enjoy!
6. A new feature is the automatic inclusion of `HEADER.md` and `README.md` file from the current directory (if any), as shown in the example above. ~~It uses [JQuery](https://jquery.com/) and [ShowDown.js](https://github.com/showdownjs/showdown/), it is not so cleanly written but it works perfectly!~~ I wanted this feature as I have it for Apache (see [this project](https://bitbucket.org/lbesson/autoindex-strapdown)).

## Configuration

A standard config looks something like this (use `-light` for the default light theme, or `-dark` for a dark theme):

```bash
fancyindex on;
fancyindex_localtime on;
fancyindex_exact_size off;
# Specify the path to the header.html and foother.html files, that are server-wise,
# ie served from root of the website. Remove the leading '/' otherwise.
fancyindex_header "/Nginx-Fancyindex-Theme-light/header.html";
fancyindex_footer "/Nginx-Fancyindex-Theme-light/footer.html";
# Ignored files will not show up in the directory listing, but will still be public.
fancyindex_ignore "examplefile.html";
# Making sure folder where these files are do not show up in the listing.
fancyindex_ignore "Nginx-Fancyindex-Theme-light";
```

If you want to conserve a few more bytes in network transmissions enable gzip on the served assets.

```bash
# Enable gzip compression.
  gzip on;

  # Compression level (1-9).
  # 5 is a perfect compromise between size and CPU usage, offering about
  # 75% reduction for most ASCII files (almost identical to level 9).
  gzip_comp_level    5;

  # Don't compress anything that's already small and unlikely to shrink much
  # if at all (the default is 20 bytes, which is bad as that usually leads to
  # larger files after gzipping).
  gzip_min_length    256;

  # Compress data even for clients that are connecting to us via proxies,
  # identified by the "Via" header (required for CloudFront).
  gzip_proxied       any;

  # Tell proxies to cache both the gzipped and regular version of a resource
  # whenever the client's Accept-Encoding capabilities header varies;
  # Avoids the issue where a non-gzip capable client (which is extremely rare
  # today) would display gibberish if their proxy gave them the gzipped version.
  gzip_vary          on;

  # Compress all output labeled with one of the following MIME-types.
  gzip_types
    application/atom+xml
    application/javascript
    application/json
    application/ld+json
    application/manifest+json
    application/rss+xml
    application/vnd.geo+json
    application/vnd.ms-fontobject
    application/x-font-ttf
    application/x-web-app-manifest+json
    application/xhtml+xml
    application/xml
    font/opentype
    image/bmp
    image/svg+xml
    image/x-icon
    text/cache-manifest
    text/css
    text/plain
    text/vcard
    text/vnd.rim.location.xloc
    text/vtt
    text/x-component
    text/x-cross-domain-policy;
  # text/html is always compressed by gzip module

  # This should be turned on if you are going to have pre-compressed copies (.gz) of
  # static files available. If not it should be left off as it will cause extra I/O
  # for the check. It is best if you enable this in a location{} block for
  # a specific directory, or on an individual server{} level.
  # gzip_static on;
```

> Reference: [H5BP Nginx Server Config](https://github.com/h5bp/server-configs-nginx/blob/master/nginx.conf)

## Examples
### Showing a list of files (without search):
![Demo #2](screenshots/Nginx-Fancyindex-Theme__example2.png "Example of Nginx-Fancyindex-Theme")

---

### Filter a list of files (with search):
![Demo #1](screenshots/Nginx-Fancyindex-Theme__example1.png "Example of Nginx-Fancyindex-Theme")

---

### Filter a list of directories (with search):
![Demo #3](screenshots/Nginx-Fancyindex-Theme__example3.png "Example of Nginx-Fancyindex-Theme")

---

### Filter a list of directories (with search) -- Dark theme:
It also shows the automatic inclusion of `HEADER.md` file and `README.md` file.

![Demo #4](screenshots/Nginx-Fancyindex-Theme__example4.png "Example of Nginx-Fancyindex-Theme-dark")

### Include `HEADER` and `README` files automatically:
Another demo:

![Demo #6](screenshots/Nginx-Fancyindex-Theme__example6.png "Example of Nginx-Fancyindex-Theme-light")

---

### :scroll: License ? [![GitHub license](https://img.shields.io/github/license/Naereen/Nginx-Fancyindex-Theme.svg)](https://github.com/Naereen/Nginx-Fancyindex-Theme/blob/master/LICENSE)
[MIT Licensed](https://lbesson.mit-license.org/) (file [LICENSE](LICENSE)).
© [Lilian Besson](https://GitHub.com/Naereen), 2016-18.

[![Maintenance](https://img.shields.io/badge/Maintained%3F-yes-green.svg)](https://GitHub.com/Naereen/Nginx-Fancyindex-Theme/graphs/commit-activity)
[![Ask Me Anything !](https://img.shields.io/badge/Ask%20me-anything-1abc9c.svg)](https://GitHub.com/Naereen/ama)
[![Analytics](https://ga-beacon.appspot.com/UA-38514290-17/github.com/Naereen/Nginx-Fancyindex-Theme/README.md?pixel)](https://GitHub.com/Naereen/Nginx-Fancyindex-Theme/)

[![ForTheBadge built-with-swag](http://ForTheBadge.com/images/badges/built-with-swag.svg)](https://GitHub.com/Naereen/)
[![ForTheBadge uses-js](http://ForTheBadge.com/images/badges/uses-js.svg)](http://ForTheBadge.com)
[![ForTheBadge uses-html](http://ForTheBadge.com/images/badges/uses-html.svg)](http://ForTheBadge.com)
[![ForTheBadge uses-css](http://ForTheBadge.com/images/badges/uses-css.svg)](http://ForTheBadge.com)
[![ForTheBadge uses-badges](http://ForTheBadge.com/images/badges/uses-badges.svg)](http://ForTheBadge.com)
[![ForTheBadge uses-git](http://ForTheBadge.com/images/badges/uses-git.svg)](https://GitHub.com/)
