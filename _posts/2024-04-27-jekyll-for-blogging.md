---
layout: post
title: "Getting Started with Jekyll for Blogging"
date: 2024-04-27 01:36:17 +0200
categories: software
tags: software
author: developer
---

## Getting Started with Jekyll for Blogging (aka. How I built this blog)

[Jekyll](https://jekyllrb.com/) is a powerful static site generator that's perfect for creating a blog. It's
simple to set up and use, and it allows you to focus on writing content without worrying about the
complexities of managing a dynamic CMS. In this tutorial, we'll walk through the process of setting up a basic
blog using Jekyll.

### Step 1: Install Ruby

Before we dive into setting up Jekyll, ensure you have Ruby installed on your system. A convenient way to
manage Ruby installations is by using RVM (Ruby Version Manager). You can install Ruby with RVM by following
the instructions [here](https://rvm.io/rvm/install).

```bash
curl -sSL https://get.rvm.io | bash -s stable
rvm install 3.0.0
rvm use 3.2.0 --default
```

### Step 2: Install Jekyll

Now, you'll need to install Jekyll. If you're using macOS or Linux, you can do this using gem, the package
manager for Ruby:

```bash
gem install jekyll bundler
```

### Step 3: Create a New Jekyll Site

Once Jekyll is installed, you can create a new Jekyll site by running the following command:

```bash
jekyll new myblog
```

Replace `myblog` with the name of your blog directory. This command will create a new directory containing a
basic Jekyll site structure.

If you want to create a blank project use:

```bash
jekyll new myblog --blank
```

### Step 4: Customize Your Blog

Navigate into your new blog directory (`cd myblog`) and open the `_config.yml` file in a text editor. This
file contains the configuration settings for your Jekyll site. You can customize settings such as the site
title, description, and other options.

### Step 5: Customize Your Theme (Optional)

Understanding Jekyll Directory Structure

- \_includes: This directory contains reusable code snippets that can be included in your pages or layouts.
  For example, navigation menus or footer sections.

- \_layouts: This directory contains layout templates for your site. Layouts define the structure of your
  pages and can be customized to create different page types (e.g., blog posts, pages).

- \_posts: This directory is where you'll store your blog posts. Each blog post should be a Markdown file with
  a filename following the format YYYY-MM-DD-title.md.

- \_data: This directory is used to store data files in formats such as YAML or JSON. You can use data files
  to store site-wide configuration, reusable content, or structured data.

- \_sass: This directory contains Sass (Syntactically Awesome Style Sheets) files, which allow you to use
  variables, mixins, and other features to simplify your CSS code.

- \_config.yml: This is the main configuration file for your Jekyll site. You can customize settings such as
  the site title, description, theme, and more.

### Step 6: Minimal files required

\_layouts/default.html

```html
{% raw %}
<!DOCTYPE html>
<html lang="{{ site.lang | default: "en-US" }}">
  <head>
    <meta charset='utf-8'>
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1">
    <link rel="stylesheet" href="{{ '/assets/css/style.css?v=' | append: site.github.build_revision | relative_url }}">
  </head>
  <body>
    <header>
      <div class="container">
        <a id="a-title" href="{{ '/' | relative_url }}">
          <h1>{{ site.title | default: site.github.repository_name }}</h1>
        </a>
        <h2>{{ site.description | default: site.github.project_tagline }}</h2>
      </div>
    </header>
    <div class="container">
      <section id="main_content">
        {{ content }}
      </section>
    </div>
  </body>
</html>
{% endraw %}
```

\_layouts/post.html

{% raw %}

```html
---
layout: default
---

<small>{{ page.date | date: "%-d %B %Y" }}</small>

<h1>{{ page.title }}</h1>

<p class="view">by {{ page.author | default: site.author }}</p>

{{content}} {% if page.tags %} <small>tags: <em>{{ page.tags | join: "</em> - <em>" }}</em></small>

{% endif %}
```

{% endraw %}

/assets/css/style.css

```scss
{% raw %}
$body-background: $cod-grey !default;
$body-foreground: $gallery !default;
$header: $conifer !default;
$blockquote-color: $silver-chalice !default;
$blockquote-border: $dove-grey !default;
$container-max-width: 1000px;

@mixin media-max-width($max-width) {
  @media (max-width: $max-width) {
    @content;
  }
}

@font-face {
  font-family: "hack";
  src: url("../fonts/Hack-Regular.ttf") format("ttf");
  font-style: normal;
  font-weight: 400;
  text-rendering: optimizeLegibility;
}

body {
  margin: 0;
  padding: 0;
  background: $body-background;
  color: $body-foreground;
  font-size: 16px;
  line-height: 1.5;
  font-family: "hack", monospace;
  color: #ccc;
}
{% endraw %}
```

index.html

{% raw %}

```html
---
layout: default
---

<ul>
  {% for post in paginator.posts %}
  <li>
    <h2><a href="{{ post.url | prepend: site.baseurl | replace: '//', '/' }}">{{ post.title }}</a></h2>
    <time datetime="{{ post.date | date_to_xmlschema }}">{{ post.date | date_to_string }}</time>
    <p>
      {{ post.content | strip_html | truncatewords:50 }}
      <a href="{{ post.url | prepend: site.baseurl | replace: '//', '/' }}"> Read more</a>
    </p>
  </li>
  {% endfor %}
</ul>
```

{% endraw %}

### Step 7: Create New Blog Posts

To create a new blog post, create a new Markdown file (`.md`) in the `_posts` directory. The filename should
follow the format `YYYY-MM-DD-title.md`, where `YYYY-MM-DD` is the date of the post and `title` is the title
of the post.

Add your blog content using Markdown syntax. For example:

```markdown
---
layout: post
title: My First Post
---

Hello, world! This is my first blog post.
```

### Step 8: Run Jekyll Locally

To preview your blog locally, run the following command from your blog directory:

```bash
bundle exec jekyll serve
```

This command will start a local web server, and you can view your blog by navigating to
`http://localhost:4000` in your web browser.

### Step 9: Deploy Your Blog

Once you're happy with your blog, you can deploy it to a web server. Jekyll generates a static site, so you
can host it on any web server that supports static files. You could also use Github Pages for free.

### Conclusion

Jekyll's simplicity and flexibility make it a great choice for creating a blog, allowing you to focus on
creating great content without getting bogged down in the complexities of managing a CMS. Happy coding!
