---
title: "Hugo Usage"
date: 2017-10-10T19:23:58+08:00
weight: 10
---

# Useful Commands

* `hugo help`: get Hugo documentation!
* `hugo`: generates the site to the destination folder
* `hugo --cleanDestinationDir`: remove files from destination not found in static directories

# Creating a New Chapter

* `hugo new --kind chapter [path to file]/_index.md`

# Creating a New Article

* Creater your own archetypes first!
* `hugo new [path to post]/[post name].md`: create a new post
* The one with the lower weight shows up first

# [Contest Organization](http://docdock.netlify.com/content-organisation/)

* All posts are contained in the `content` folder
* The folder hierarchy is the shape of the website

    ```
    content
    ├── level-one
    │   ├── level-two
    │   │   ├── level-three
    │   │   │   ├── level-four
    │   │   │   │   ├── _index.md
    │   │   │   │   ├── page-4-a.md
    │   │   │   │   ├── page-4-b.md
    │   │   │   │   └── page-4-c.md
    │   │   │   ├── _index.md
    │   │   │   ├── page-3-a.md
    │   │   │   ├── page-3-b.md
    │   │   │   └── page-3-c.md
    │   │   ├── _index.md
    │   │   ├── page-2-a.md
    │   │   ├── page-2-b.md
    │   │   └── page-2-c.md
    │   ├── _index.md
    │   ├── page-1-a.md
    │   ├── page-1-b.md
    │   └── page-1-c.md
    ├── _index.md
    └── page-top.md

    ```
* Make sure that `_index.md` is present!

# Tips for Writing Markdown 

Hugo Docdock theme provides a lot of shortcodes! :) Here are a few useful ones:

* To be continued...