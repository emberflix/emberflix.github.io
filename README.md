# Emberflix blog

## Setup

1. Install Jekyll (use the command `sudo gem install jekyll`)
1. Fork the repo
1. Clone the repo you just forked
1. Edit `_config.yml` to personalize your site
1. Check out the sample posts in `_posts` to see examples for assigning category, tags, image and other YAML data
1. Read the documentation below for further customization pointers and documentation
1. Remember to compile your assets files with Gulp


## Creating drafts

You can use the `initdraft.sh` to create your new drafts. Just follow the command:

```
./initdraft.sh -c Post Title
```

The new file will be created at `_drafts` with this format `date-title.md`.

## Creating posts

You can use the `initpost.sh` to create your new posts. Just follow the command:

```
./initpost.sh -c Post Title
```

The new file will be created at `_posts` with this format `date-title.md`.

## Front-matter

When you create a new post, you need to fill the post information in the front-matter, follow this example:

```
---
layout: post
title: "Welcome"
description: Lorem ipsum dolor sit amet, consectetur adipisicing elit.
image: 'http://res.cloudinary.com/dm7h7e8xj/image/upload/c_scale,w_760/v1504807239/morpheus_xdzgg1.jpg'
category: 'blog'
tags:
- blog
- jekyll
twitter_text: Lorem ipsum dolor sit amet, consectetur adipisicing elit.
introduction: Lorem ipsum dolor sit amet, consectetur adipisicing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua.
---
```

**Your image size should have the proportion of a 600x315 image to look good on home page.**

## Run locally

In order to compile the assets and run Jekyll on local you need to follow those steps:

- Install [NodeJS](https://nodejs.org/) (remember to use the latest version)
- Run `sudo npm install`
- Run `sudo npm install -g gulp gulp-cli`
- Run `sudo gulp`


## Credits

[Thiago Rossener](https://www.rossener.com/) based on [Cards Jekyll Template](https://github.com/willianjusten/cards-jekyll-template).

## License

*Jekflix Template* is available under the MIT license. See the LICENSE file for more info.
