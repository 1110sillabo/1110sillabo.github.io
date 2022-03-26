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

## Good news

I've read that \_config.yml files are not reloaded automatically (rerunning the build is required). But it seems that pushing new stuff on git triggers the build. So that's it.

# Editing the about

[This page](https://www.section.io/engineering-education/build-a-jekyll-site/) helped me fixing the about. It's mainly about the markdown and \_config.yml files having the control rather then the .html files.

I like this, but still I have to take full advantage of this.

## False Starts

- editing the index.html files for the about using the {{ site.title }} and other references to variables set in config.

# Deleting content/editing content

Markdown comes first. Edit the markdown to edit. Cancel the markdown to see the posts go away.

# Display the categories

[This gist](https://gist.github.com/Phlow/a0e3fa686eb259fe7f76) was enough to get all the categories to show up.
All I have to add was an html tag in the markdown, otherwise the code specs run (i.e.: all categories are selected, etc, all the {{tags}} are resolved and process, but the html tags will be written rather than compiled).

Still, this was not enough to add to the home.

# Adding layouts and editing home

To have the categories showing up in the main page as its content I realized I need to change Jekyll layout.
Adding what I need in the about.markdown did not work. You get the text, but it comes before everything else.

So here is the [folder structure of Jekyll themes and layout](https://jekyllrb.com/docs/themes/#overriding-theme-defaults) and [here is the home sourcecode](https://github.com/jekyll/minima/blob/master/_layouts/home.html).

## Back to Categories

As I managed to include the categories in the home I realized there was something wrong with the mechanism.
Categories in the gist are provided as a set of two lists. The first has the categories added and the count. Clicking on them sends you to the list of the category with links to the pieces tagged with that category.
Now you click on them and can reach the pieces.

**Issue with the URLs**

Probably due to the way post.url and site.url works on the github powered engine I had to remove the site.url tag to create the correct link to the post. So I have this:

<li><a href="{{ post.url }}">{{ post.title }} <time datetime="{{ post.date | date_to_xmlschema }}"
                    itemprop="datePublished">{{ post.date | date: "%B %d, %Y"
                    }}</time></a></li>

# Google Analytics (Includes)

The minima theme includes also [a dedicated file to add Google Analytics](https://github.com/jekyll/minima/blob/master/_includes/google-analytics.html).

Taking a look at the code we see this:

**https://www.googletagmanager.com/gtag/js?id={{ site.google_analytics }}**

So we need to set a google_analytics var in the config.yml file.

# Add a new section with permalink

I want to add a further section next to the about one. I want it to point to my Digital Humanities book.

[Stackoverflow helped with buttons](https://stackoverflow.com/questions/40688633/how-can-i-add-a-button-in-a-md-file-with-jekyll) but the issue was that I needed to find the right place to put the button.

Again, I had to check the \_includes and add the header.html file and customize it. Right before the loop that watches for your file and build sections out of it, you can add what you need. Cool.

## Build error

If you use the include tag somewhere in your layouts or site and don't have the corresponding html file in the \_includes folder you get an error and the build fails.

# Editing the Minima template

Once you have a minimal grasp of how this 'Jekyll thing and pages' works the best place to moove to the next tinkering stage is [Minima's repo itself](https://github.com/jekyll/minima#home-layout). That's **the** palce to figure out what's included in the \_include and \_layouts and provides all you need to start messing around with the template.

Just first copy what you need and then edit it.

# To Do

- [x] display categories
- analytics
- [x] adding more sections
- [x] try to delete a post
- [ ] handle redirects etc (will it require to check posts and categories again?)

# Catogeries experiment

Will this work?

<h2>Categories</h2>

Test compiles but it's not rendered

<html>
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

</html>
