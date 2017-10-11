---
title: "Hugo Usage"
description: "My Hugo handy refernece"
date: 2017-10-10T19:23:58+08:00
tags: ["Hugo", "Setup"]
categories: ["Hugo"]
weight: 10
---

## Useful Commands

* `hugo help`: get Hugo documentation!
* `hugo`: generates the site to the destination folder
* `hugo --cleanDestinationDir`: remove files from destination not found in static directories

## Creating a New Chapter

* `hugo new --kind chapter [path to file]/_index.md`

## Creating a New Article

* Creater your own archetypes first!
* `hugo new [path to post]/[post name].md`: create a new post
* The one with the lower weight shows up first

## [Content Organization](http://docdock.netlify.com/content-organisation/)

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

## [Tips for Writing Markdown ](https://learn.netlify.com/en/cont/markdown/)

Hugo Docdock theme provides a lot of shortcodes! :) Here are a few useful ones:

* [Codeblocks with line number](https://gohugo.io/content-management/syntax-highlighting/)
    * Use the following snippet
        ```
        {{</* highlight cpp "linenos=inline" */>}}
        {{</* / highlight */>}}
        ```
* [Alerts](http://docdock.netlify.com/shortcodes/alert/)

    ```
    {{%/* alert theme="info" */%}}**info**{{%/* /alert */%}}
    {{%/* alert theme="success" */%}}**success**{{%/* /alert */%}}
    {{%/* alert theme="warning" */%}}**warning**{{%/* /alert */%}}
    {{%/* alert theme="danger" */%}}**danger**{{%/* /alert */%}}
    ```
    {{% alert theme="info" %}}**info**{{% /alert %}}
    {{% alert theme="success" %}}**success**{{% /alert %}}
    {{% alert theme="warning" %}}**warning**{{% /alert %}}
    {{% alert theme="danger" %}}**danger**{{% /alert %}}
* [Attachment](http://docdock.netlify.com/shortcodes/attachments/)
    * markdown file: attachements must be place in a folder named like your page and ending with `.files`
    * folder: attachements must be place in a nested `files` folder.
    * Usage:

        ```
        {{%/*attachments title="Related files" pattern=".*(pdf|mp4)"/*/%}}
        ```
* [Expand](http://docdock.netlify.com/shortcodes/expand/)

    ```
    {{%/* expand "Hint ?" */%}}Here you go!{{%/* /expand*/%}}
    ```

    {{% expand "Hint" %}}Here you go!{{% /expand %}}
* [Notice](http://docdock.netlify.com/shortcodes/notice/)
    * Must be padded to the left for the title to show up!

    ```
    {{%/* notice note */%}}
    Note 
    {{%/* /notice */%}}
    ```
{{% notice note %}} 
Note
{{% /notice %}}

    ```
    {{%/* notice info */%}}
    Info 
    {{%/* /notice */%}}
    ```
{{% notice info %}} 
Info
{{% /notice %}}

    ```
    {{%/* notice tip */%}}
    Tip 
    {{%/* /notice */%}}
    ```
{{% notice tip %}} 
Tip
{{% /notice %}}

    ```
    {{%/* notice warning */%}}
    Warning
    {{%/* /notice */%}}
    ```
{{% notice warning %}}
Warning
{{% /notice %}}

* [panel](http://docdock.netlify.com/shortcodes/panel/)
    * success, default, primary, info, warning, danger

    ```
    {{%/* panel theme="success" header="Title" */%}}content{{%/* /panel */%}}
    ```
{{% panel theme="success" header="Title" %}}content{{% /panel %}}

* [revealjs](http://docdock.netlify.com/shortcodes/revealjs/)