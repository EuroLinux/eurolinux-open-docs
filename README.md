# EuroLinux


## About Documentation
This is EuroLinux community-driven documentation.

**We welcome your contributions to EuroLinux!**

You can:

- star the repository to show your support
- contribute via a Pull Request - see [Contribution Guide](#contributors-guide)
- create requests for a particular topic via [Issue Creation on
  GitHub](https://github.com/EuroLinux/eurolinux-open-docs/issues/new/choose)



!!! info additional documentation
    As EuroLinux is in Open Core model there are also additional documentation
    for our customer that are available at [EuroLinux Support
    Portal](https://support.euro-linux.com).

## How documentation is organized?

Documentation is organized in the following manner:

- JumpStarts - Installation guides with extras
- HowTo - How To guides on various topics
- Erratas - EuroLinux erratas in the form of the web pages
- Release Notes 
 
## Contributors guide
### How to contribute
TODO
 Step - by -step instruction (make fork, PR etc)


### Tools
We are using `mkdocs` with `mkdocs-material` to build and style our
documentation.

- [MkDocs site](https://mkdocs.readthedocs.io/en/stable/)
- [Material for MkDocs site](https://squidfunk.github.io/mkdocs-material/)


### Setup environment locally

Because MkDocs is Python based, you need at least these installed to run this
documentation locally:

- python3 (3.6+)
- pip
- virtualenv

First, let's create a virtualenv, so you don't bloat your system-wide python
environment:
```
virtualenv -p /usr/bin/python3 venv
```

Then activate virtualenv

Bash:
```bash
. venv/bin/activate
```

Fish:
```fish
. venv/bin/activate.fish
```

Now you are ready to install MkDocs and other Python packages:
```
pip install -r requirements.txt
```

After it serving documentation on your host is as easy as running:
```
mkdocs serve
```

To build documentation invoke:
```
mkdocs build
```

It will build documentation and save it into `site` directory

!!! warning "Please don't include site directory in pull requests"
    Because we deploy this documentation with GitHub Pages, the `site`
    directory is not gitignored


### Markdown cheat sheet for this project
We created simple cheat sheet for MkDocs markdown syntax with extensions
enabled in this project. It can be found
[here](HowTo/z-documentation-markdown.md).
