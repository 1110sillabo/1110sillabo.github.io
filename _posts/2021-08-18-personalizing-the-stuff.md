---
layout: post
title: "Personalizing the stuff"
date: 2021-08-18 18:34:ss -0000
categories:
  - building
  - notes-to-future-self
---

# Overview

Installing Ruby and Jekyll was not such a big issue.
But it took me quite sometime to start experimenting with the feel of writing a managing a blog from here.

These are notes to future self to track what's going on and how it feels.

# Customizing Jekyll

Starting a Jekyll blogs gives you a home, an about section and some posts.
Most of the defaults are edited through the \_config.yml settings. Adding a post is a matter of markdown and [github docs contents](https://docs.github.com/en/pages/setting-up-a-github-pages-site-with-jekyll/adding-content-to-your-github-pages-site-using-jekyll) have all the info to get started and add some content (like these post).

# Editing the about

[This page](https://www.section.io/engineering-education/build-a-jekyll-site/) helped me fixing the about. It's mainly about the markdown and \_config.yml files having the control rather then the .html files.

I like this, but still I have to take full advantage of this.

## False Starts

- editing the index.html files for the about using the {{ site.title }} and other references to variables set in config.

# Good news

I've read that \_config.yml files are not reloaded automatically (rerunning the build is required). But it seems that pushing new stuff on git triggers the build. So that's it.

# To Do

- display categories
- analytics
- adding more sections
- try to delete a post:

# Catogeries experiment

Will this work?

<h2>Categories</h2>
        <ul>
          {% assign categories_list = site.categories %}
          {% if categories_list.first[0] == null %}
          {% for category in categories_list %}
          <li><a href="#{{ category | downcase | downcase | url_escape | strip | replace: ' ', '-' }}">{{ category |
              camelcase }} ({{ site.tags[category].size }})</a></li>
          {% endfor %}
          {% else %}
          {% for category in categories_list %}
          <li><a href="#{{ category[0] | downcase | url_escape | strip | replace: ' ', '-' }}">{{ category[0] |
              camelcase }} ({{ category[1].size }})</a></li>
          {% endfor %}
          {% endif %}
          {% assign categories_list = nil %}
        </ul>

        {% for category in site.categories %}
        <h3 id="{{ category[0] | downcase | url_escape | strip | replace: ' ', '-' }}">{{ category[0] | camelcase }}
        </h3>
        <ul>
          {% assign pages_list = category[1] %}
          {% for post in pages_list %}
          {% if post.title != null %}
          {% if group == null or group == post.group %}
          <li><a href="{{ site.url }}{{ post.url }}">{{ post.title }} <time
                datetime="{{ post.date | date_to_xmlschema }}" itemprop="datePublished">{{ post.date | date: "%B %d, %Y"
                }}</time></a></li>
          {% endif %}
          {% endif %}
          {% endfor %}
          {% assign pages_list = nil %}
          {% assign group = nil %}
        </ul>
        {% endfor %}
