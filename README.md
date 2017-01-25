# Shippable Documentation

## Overview

Shippable is a DevOps platform that lets you automate your end to end pipeline including [CI/CD and Release Automation](https://app.shippable.com). This repository contains the documentation, which is open source to allow anyone to make quick changes if needed.

The source is under `sources/` in the form of .md files. These files use [Markdown](http://daringfireball.net/projects/markdown/syntax) formatting with [Mkdocs](http://www.mkdocs.org/) extensions for structure, cross-linking and some Markdown additional syntax using [Python-Markdown](https://pythonhosted.org/Markdown/extensions/).

The HTML files are built and hosted on [Amazon S3](http://aws.amazon.com/s3/), appearing via proxy on [docs.shippable.com](http://docs.shippable.com/). The HTML files update automatically after each change to the master.

## Contributing

### Using GitHub web interface

For small changes and typos you might want to use
GitHub's built in file editor. It allows you to preview your changes
right online (though there can be some differences between GitHub
flavored markdown and Mkdocs markdown). Just be careful not to create many commits.

### Using local copy of the repository

#### Installation

To get started you have to have git and python3 with pip and virtualenv installed:

```bash
$ git clone git@github.com:Shippable/docsv2.git shippable-docs
$ cd shippable-docs
$ virtualenv venv
$ source venv/bin/activate
(venv)$ pip install -r requirements.txt
```

#### Server

To start documentation server at `localhost:5555`:

```bash
(venv)$ mkdocs serve --dev-addr=localhost:5555
```
It uses livereload there is no need to restart the server after changes have been made. Also it automatically reloads pages in the browser.

#### Structure

Page structure is really easy to change with mkdocs:

```yaml
# mkdocs.yaml

pages:
  - [index.md, Overview]

  - [start.md, Getting Started]

  - [yml.md, Configuration, Configuring Your YML]
  - [config.md, Configuration, A Bit Deeper]
```

> http://www.mkdocs.org/user-guide/configuration/#pages

#### Images

When you need to add images, try to make them as small as possible
(e.g. as gif). Usually images should go to the same directory as the
.md file which references them (`![Alt](path.ext)`), or in a subdirectory if one already
exists.

#### Deploy

Just commit and push you changes to the master branch to update documentation site.

## Settings

To configure deployment change `shippable.yml`:

```yaml
# shippable.yml

env:
  global:
    - AWS_S3_LOCAL_PATH='site'
    - AWS_S3_BUCKET='s3://<bucket>'
    - AWS_S3_REGION='<region>'
    # AWS_ACCESS_KEY_ID
    # AWS_SECRET_ACCESS_KEY
    - secure: <encrypted>

notifications:
  email:
    recipients:
      - <email>
```

To update theme change files in `theme/shippable` directory.

> During sphinx-mkdocs migration `theme/shippable/static/ash`
> files was handled as legacy code and never have been changed except:
>
> - `theme/shippable/static/ash/js/custom.js`
> - `theme/shippable/static/ash/css/custom.css`
>
> All html files and those two files look like good targets for theme tweaking.

## Issues

- A broken link in api.md: `https://prod-shippable.s3.amazonaws.com/artifacts/subscriptions/.../tar.gz`
- <a target="_blank" href="http://www.mkdocs.org/">Mkdocs</a> for now (May 2015) is a real raw project without enough documentation
- "Edit this article" doesn't point to the document only to the whole source dir:
    - https://github.com/mkdocs/mkdocs/issues/269
    - https://github.com/mkdocs/mkdocs/issues/266
- There are still 2 javascript errors (as before migration):
    - related to jsapi
    - related to async-ads.js
- No text in 404.html because of bug with including content block
- Header menu doesn't work on mobile (as before migration)
- Bootstrap theme "Little Necko" patches bootstrap and does it in really odd way. Some hacks are used to fight "Little Necko". All css looks legacy and fragile. Hopefully css will be rewritten from strach soon.
- Minor style bugs (especially for mobile)

> As default in boostrap, shippable and docs.shippable use following font stack:
> "Helvetica Neue", Helvetica, Arial, sans-serif. Helvetica Neue is proprietary
> font is not available on most Linux ans Windows machines (that's true right?).
> Also it can't be shipped with website as webfont because of its license. That
> means Helvetica font for the most of the users which someone believe a little bit
> ugly font for the web.
