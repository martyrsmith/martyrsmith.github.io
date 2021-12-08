---
title: How it's made
date: 2021-11-22 11:20:00
tags:
- hexo
- github
header_image: /intro/index-bg.jpg
categories:
  - blog
---
Today we describe how this website is created and updated. We use a Hexo framework hosted on Github. 
<!-- more -->

## Hexo

Hexo is our blogging framework. 

### Setup
* [Setup Docs](https://hexo.io/docs/setup.html)

### Themes
* [Theme Docs](https://hexo.io/docs/themes)

### Markdown image support
* [Hexo-asset-link](https://github.com/liolok/hexo-asset-link)
* [Adding Images to Hexo Posts with Markdown](https://chrismroberts.com/2020/01/06/using-markdown-in-hexo-to-add-images/)

### Folder structure
```
├── _drafts
└── _posts
   ├── how-its-made
   │   ├── example.drawio
   │   └── example.drawio.svg
   └── how-its-made.md
```

## Github

Github stores our site's code and hosts the statically generated website. 

### Actions
* [Actions docs](https://docs.github.com/en/actions/quickstart)

#### Example
This automatically builds Hexo site from [main] branch and publishes to [gh-pages] brach.

The secret [secrets.GITHUB_TOKEN] is provided by default to the all running jobs.

```
name: Pages

on:
  push:
    branches:
      - main  # default branch

jobs:
  pages:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - name: Use Node.js 12.x
        uses: actions/setup-node@v1
        with:
          node-version: '12.x'
      - name: Cache NPM dependencies
        uses: actions/cache@v2
        with:
          path: node_modules
          key: ${{ runner.OS }}-npm-cache
          restore-keys: |
            ${{ runner.OS }}-npm-cache
        - name: Install Dependencies
        run: npm install
      - name: Build
        run: npm run build
      - name: Deploy
        uses: peaceiris/actions-gh-pages@v3
        with:
          github_token: ${{ secrets.GITHUB_TOKEN }}
          publish_dir: ./docs
          publish_branch: gh-pages  # deploying branch
          cname: dbnservers.net

```

### Pages
* [Pages docs](https://pages.github.com/)
* Configure to watch branch [gh-pages] root [/root] folder.
* Setup a custom domain and SSL

## Diagrams.net
Used to create diagrams. 

### Create new diagram
* Download desktop client from [diagrams.net](https://www.diagrams.net/blog/diagrams-offline)
* New diagram
* Navigate to post folder [source/_posts/how-its-made]
* Name [example.drawio]

### Export as SVG
* File, Export as, SVG
* Navigate to post folder [source/_posts/how-its-made]
* Name [example.drawio.svg]


## Markdown
Formatting sytax for blog posts.

[Cheatsheet](https://enterprise.github.com/downloads/en/markdown-cheatsheet.pdf)


### Example image code
```
![simple diagram](how-its-made/example.drawio.svg)
```

### Example image rendered
![simple diagram](how-its-made/example.drawio.svg)

