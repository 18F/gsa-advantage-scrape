# GSA Advantage Cart Scrape

This is a really simple script to scrape a parked cart from GSA Advantage. Thaks to [Sean Herron](https://github.com/seanherron) for doing the breakthrough work. I have just filled in details.

## Reason for Existence

To some extent this is experimental. If you aren't a Federal government user of GSA Advantage!
or have to wish to server those users, this code is only valuable to use as an example, if that.

We are building a "spike" that re-utilizes GSA Advantage's excellent shopping cart building
capabilities.  Our goal is to build a searchable database of shopping carts and to smooth
the process of communication around carts.  This project could be considered a part of the project
here at github codenamed "mario" (https://github.com/18F/Mario), but I am making this repo stand-alone in hopes of greater
reuse.

We of course invite assistance, but this is not the best project to help on, due to its
very "spikey" nature, so I am not opening any issues at present---but improving my Python code
is generally easy to do!

## Installation

```bash
# optional - install within VirtualEnv
cd src
virtualenv gsascraper
cd ..

pip install -r requirements.txt
```

## Usage

To make this useful, we support command-line, CGI, and Flask usage.

### Command-Line

The command line usage is simple:

```
python gsa-scrape-commandline.py
```

### CGI

However, we normally used this a web-service API, and therefore host
it with Apache.  Although there are many ways to do this, I prefer to
do it as a virtual host.  This requires two steps:  Adding an entry to
`/etc/hosts` file and adding a VirtualHost entry to your apache
configuration.

This is the entry I add to `/etc/hosts`, which allow the url
http://gsa-advantage-scraper to be resolved to localhost.

```
127.0.0.1       gsa-advantage-scraper
```

The example configuration looks like this, although you may prefer
not to use port 80, which makes more sense if you do not which to
expose this service outside the machine on which you have installed
it.  If you, for example, install it on 8080, be sure to configure
Apache to listent on that port.  Be sure that the CGI handler is installed.

```xml
<VirtualHost gsa-advantage-scraper:80>
    ServerName gsa-advantage-scraper
    ScriptAlias /cgi-bin/ /Users/robertread/projects/gsa-advantage-scrape/src/ \

      DocumentRoot /Users/robertread/projects/gsa-advantage-scrape/src
       <Directory "/Users/robertread/projects/gsa-advantage-scrape/src">
            Options +ExecCGI -MultiViews +SymLinksIfOwnerMatch
            Order allow,deny
            AllowOverride None
            Allow from all
            DirectoryIndex index.php
            AddHandler cgi-script .py .cgi
      </Directory>
</VirtualHost>
```

Depending on whether have used the "VirtualEnv" installation method
above or have installed your python packages where they can be reached
without VirtualEnv, you should access `gsa-adv-cart-ve.py` or
`gsa-adv-cart.py`.

### Flask App

To make the API available via a Flask app, run:

```bash
python src/server.py
```

then do requests to http://127.0.0.1:5000/api/v1/carts/CART_ID?u=USER&p=PASSWORD. Note this endpoint supports JSONP via a `callback` parameter.

### Note

The scraper is best tested by either using Curl or by using the
commandline program included.  If you know all of the necessary
credentials, the commandline interface is the best way to insure that
you have all Python modules installed.

Curl is the best way to make sure that you have Apache configured as
you desire.

The most common usage of this project is to serve as an API used by
the project [Mario](https://github.com/18F/Mario]) but you of course may use it as you see fit.

## Public Domain

This project is in the worldwide [public domain](LICENSE.md). As stated in [CONTRIBUTING](CONTRIBUTING.md):

To test, you can run "python gsa-advantage-py".

### Public domain

This project is in the worldwide [public domain](LICENSE.md). As stated in [CONTRIBUTING](CONTRIBUTING.md):

> This project is in the public domain within the United States, and copyright and related rights in the work worldwide are waived through the [CC0 1.0 Universal public domain dedication](https://creativecommons.org/publicdomain/zero/1.0/).
>
> All contributions to this project will be released under the CC0 dedication. By submitting a pull request, you are agreeing to comply with this waiver of copyright interest.
