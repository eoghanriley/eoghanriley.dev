---
layout: ../../../layouts/Blog.astro
title: 'Writing a blog/portfolio'
date: 2022-8-9
description: 'test blog post'
categories: ['test']
length: '1 minute'
---

# Introduction

I have wanted/needed a portfolio for a while but I haven't out of lack of things to put on said site.
A few months ago I decided to set out on making a portfolio with hugo but, I ended up quitting after hours of struggling to get a theme working properly.
However recently I got a wave of motivation since I need a portfolio and wanted a blog for my pygame framework that I'm working on.

# Finding a theme

So with my wave of motivation I went to the hugo site to look for themes that can serve as a blog and portfolio. I found a few themes that I liked but they were only one or the other. Eventually I found a theme that fit my needs and I liked it so I tried to set it up after about an 1 hour of debugging the theme would not work. So I went back to finding a new theme until I found this theme Gokarna.

# Setup

Unlike the 2 previous themes I attempted this theme was essentially plug and play to get the homepage working I just needed to add

```
theme = 'gokarna'
```

and then it just worked. Perfect.

# Why Hugo?

While I like making websites in fact one of the projects I'm working on right now is in svelte, it's just nice to be able to make this blog at the end of the day without having to write a lot of code. The purpose of this site is for me to be able to write about my side projects and not to make another side project. Thats the beauty of hugo I can make a beautiful site where the only thing I have to write is just markdown.

# Configuring the theme

The nice thing about Gokarna is that it didnt need me to do a lot of configuring to the base functionality working.
I just need to write a few lines to get the header icons done, the name, the description, etc.
All of this was really just adding things to the config.toml.
The only thing that was not, was in order to the dark mode and light mode selector to work I had to go into the themes javascript and change one line from light to dark.

# Adding posts

Making blog posts is really easy well minus writing the content.
I can create the file with

```
hugo new posts/name.md //this includes the hugo metadata
```

versus

```
touch content/posts/name.md //this does not include the hugo metadata
```

Then I open my editor of choice (nvim of course) then start writing away.

# Creating Projects

The projects page was really easy to make.
After running

```
hugo new projects/_index.md
```

I created the file I needed then I added a header and a link and it was done.
Heres the code for those that want it. It's also on github if you want to look at the rest of the code for the site.

```
---
title: 'Projects'
type: page
draft: false
---

## My Projects :)

[This Site](/posts/writiting-a-blog-portfolio)

[Untitled Framework](/tags/untitled-framework/)
```

# Wrapup

In the end my experience has been very easy in Hugo (as long as you pick the right theme). In the future I may eventually work on my own theme that fits my needs better but for now its fine.
I think Hugo would be a good introduction to webdev to anyone interested because you can write some code if you want you can also not.
While you get more familiar with hugo you can also slowly ease yourself into writing html, css, js etc.
