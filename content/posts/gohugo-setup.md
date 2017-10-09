---
title: "Gohugo Setup"
date: 2017-10-09T23:30:04+08:00
draft: false
---

[GoHugo](https://gohugo.io) is a very well-designed static site generator. The user experience is way better than what I have tried so far. One of the good things about Hugo is that the config file is very clean! Also, the installation is easy and version controlling it is quite simple.

# Install GoHugo

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
5. Start the development server: `hugo server -D`
6. In `config.toml`, customize the site URL, title, etc.
7. In order to let Github Pages to parse from [`docs` folder](https://gohugo.io/hosting-and-deployment/hosting-on-github/#deployment-via-docs-folder-on-master-branch):
    * add `publishDir = "docs"` to `config.toml`
    * add `publishDir: docs` to `config.yaml`
8. Run `hugo` to build it!
9. Push the generated static pages to Github, and wait for Github Pages to build! Enjoy!! It's that easy!

# Cloning Hugo Site from Github

1. Make sure you have hugo installed. Installation can be checked by running `hugo help`
2. Clone the repo
3. Run `git submodule init` on project root and then run `git submodule update` to get the theme
4. Run `hugo server`! You are done setting up!
