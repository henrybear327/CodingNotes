---
title: "Hugo Setup"
date: 2017-10-09T23:30:04+08:00
draft: false
tags: ["Hugo", "Setup"]
categories: ["Hugo"]
weight: 5
---

[Hugo](https://gohugo.io) is a very well-designed static site generator. The user experience is way better than what I have tried so far. One of the good things about Hugo is that the config file is very clean! Also, the installation is easy, version controlling it is quite simple, and it's easy to share the source code and let people create post PR!

<!--more-->

## Install and Setup Hugo

The [quickstart guide](https://gohugo.io/getting-started/quick-start/) on Hugo is pretty comprehensive. Please check it out!

1. Install latest hugo version
    * On Linux: `sudo snap install hugo`
    * On Mac: `brew install hugo`
2. Create a hugo site: `hugo new site [sitename]`
3. By default, no theme is installed, so you need to install one on your own!

    ```bash
    cd [sitename];\
    git init;\
    git submodule add https://github.com/budparr/gohugo-theme-ananke.git themes/ananke;\

    # Edit your config.toml configuration file and add the Ananke theme.
    echo 'theme = "ananke"' >> config.toml
    ```

    Using [git submodule](https://git-scm.com/book/zh-tw/v1/Git-工具-子模組-Submodules) is a very good idea since we can keep track of the separate projects version controlled by git easily.

4. Add some content: `hugo new posts/my-first-post.md`. Notice that the draft mode by default is true, so it won't be published unless you change it to false.
    * [`.rst`](http://docutils.sourceforge.net/docs/user/rst/quickref.html) will [work](https://gohugo.io/content-management/formats/#additional-formats-through-external-helpers)!
5. Start the development server: `hugo server -D`
6. Open `config.toml`
    * customize the site URL, title, etc.
    * `.toml` [reference](https://github.com/toml-lang/toml)
7. In order to let Github Pages to parse from [`docs` folder](https://gohugo.io/hosting-and-deployment/hosting-on-github/#deployment-via-docs-folder-on-master-branch):
    * add `publishDir = "docs"` to `config.toml`
    * add `publishDir: docs` to `config.yaml`
8. Run `hugo` to build it!
9. Push the generated static pages to Github, and wait for Github Pages to build! Enjoy!! It's that easy!

## Cloning Hugo Site from Github

1. Make sure you have hugo installed. Installation can be checked by running `hugo help`
2. Clone the repo
3. Run `git submodule init` on project root and then run `git submodule update` to get the theme
4. Run `hugo server`! You are done setting up!

## Switching to [DocDock Theme](http://docdock.netlify.com/getting-start/installation/) + My Personal Tweaks

This theme provides searching, tagging, and lot's of shortcodes out-of-the-box. Worth a try!

1. Get the theme code: `git submodule add https://github.com/vjeantet/hugo-theme-docdock.git themes/docdock`
2. Use the theme's config file: `cp themes/docdock/exampleSite/config.toml .`:
    * Better run `cp config.toml config.toml.bak` first
3. Open `config.toml`:
    * Set `theme = "docdock"`
    * Comment out `themesdir = "../.."`
4. Create a `_index.md` document in the content folder
    * [Don't forget to create](http://docdock.netlify.com/content-organisation/) a `_index.md` with `title:` and `head:` front matter!
5. Update files in `archetypes` folder:
    * Check [front matter](https://gohugo.io/content-management/front-matter/) format
    * Tweak and add your own files! It's will be very handy to have some of your own!
6. Create a `_header.md` page in content folder. Its content is what you get in the logo placeholder (top left of the screen).

## MathJax support

1. Create a `custom-head.html` into a `layouts/partials` folder next to the content folder, this is where we [should add](http://docdock.netlify.com/content-organisation/customize-style/) the javascript code in every `<head>`. Paste the [following code](http://docs.mathjax.org/en/latest/start.html) in to the file in order for MathJax and inline latex to work!

    ```javascript
    <!-- Partial intended to be overwritten to add CSS -->
    <script type="text/x-mathjax-config">
        MathJax.Hub.Config({
        tex2jax: {inlineMath: [['$','$'], ['\\(','\\)']]}
        });
    </script>
    <script type="text/javascript" async
    src="https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.2/MathJax.js?config=TeX-MML-AM_CHTML">
    </script>
    ```
    Also, if you have Google Analytics tracking code, you can add it here.
2. Check out the result $e = mc^2$ :)