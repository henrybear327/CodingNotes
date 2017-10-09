---
title: "Gohugo Setup"
date: 2017-10-09T23:30:04+08:00
draft: false
---

# [Install GoHugo](https://gohugo.io/getting-started/quick-start/)

The quickstart guide on Hugo is pretty comprehensive. Please check it out!

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

    One good thing about Hugo is that the config file is very clean!

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
