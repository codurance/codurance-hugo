# HUGO - a static site generator

# How does it work?

Hugo is a static site generator. Hugo generates the pages in the similar way as Jekyll. 

Users prepare the content files written in the Markdown format. Each content file contains also metadata added as a front matter to the content file.
Metadata is used by Hugo to define data available in templates which generate the site. 

During the generation of the page Hugo combines the content files, a theme and **the site config file** and based on these elements Hugo produces the HTML files. 
**The theme** defines the layout of the page by introducing the set of template files written in the Golang Templates format.  

If you don't change your layout, all you need to do is to create/modify content files.

# The content

Hugo is the content-centric site generator. Each page in the target site should have a corresponding content file.
Hugo assumes that your site will be organized into sections and each section will use the corresponding type.
Each section can contain different types of content, but the content file can have only one type.
Each type can have different layouts, but the content file can have only one layout. 
  
Content files should be stored in the directory named `content`. The structure of the `content` directory determines the target URL of the content.

The URL contains the following elements:

- **baseURL** - the base URL of the site defined in the *the site config file* 

- **section** - the `urlized` name of the first-level directory in the `content` directory.

- **slug** - the `urlized` name of the file without the extension.
 
- **path** - the `urlized` combination of **the section** and the all names of non-first-level directories on the path to the file.
 
**The URL structure**

```console
               permalink
⊢-----------------^----------------⊣
http://codurance.com/blog/some_post/

       baseURL        section       slug
⊢---------^--------⊣ ⊢--^--⊣ ⊢------^------⊣
http://codurance.com/services/example_service/

       baseURL        section                 slug
⊢---------^--------⊣ ⊢--^--⊣           ⊢------^------⊣
http://codurance.com/services/training/example_training/

       baseURL               path            slug
⊢---------^--------⊣ ⊢-------^------⊣ ⊢------^------⊣
http://codurance.com/services/training/example_training/

       baseURL                      url
⊢---------^--------⊣ ⊢----0---------^----------------⊣
http://codurance.com/services/training/example_training/

```

**The organisation of the `content` directory** 

```console
.
└── content
    ├── post
    |   ├── firstpost.md   // <- http://codurance.com/post/firstpost/
    |   ├── happy
    |   |   └── ness.md    // <- http://codurance.com/post/happy/ness/
    |   └── secondpost.md  // <- http://codurance.com/post/secondpost/
    └── services
        ├── first.md       // <- http://codurance.com/services/first/
        └── second.md      // <- http://codurance.com/services/second/
```

More details about the organisation of the files you can find [here](https://gohugo.io/content/organization/).

Each content file contains two elements:

- **front matter** - the metadata of the content in the JSON, TOML or YAML format.

- **content** - the content of the page in Markdown format. 

In **the front matter** we can define:
 
- **title** - the title of the page

- **date** - the date (and optionally time) of the publication

- **type** and **layout** - elements allowed to define the layout of the page.

- ***user data*** - key/value pairs which define additional user-defined parameters e.g. an author of a post.

```markdown
---
title: Title of the post 
date: 2016-05-15T00:01:00Z
author: Unknown

type: blog
layout: post
slug: slug-for-the-post
---

## Some header
Content of the page in the Markdown format.

```

# The theme

To generate the site in the right layout Hugo uses themes. The theme contains templates files which define how each type of content should be displayed.  
Themes should be stored in the `themes` directory.

```console
.
└── content
└── themes
    └── ...
```

Everyone can create its own theme. In our case we created our own theme to generate our site and we named this theme `codurance`. 

You can define which theme should be used during the generation, in two different ways. The first one is to define the main theme in the site configuration file (config.toml).
The other way is it define this as a parameter in the command line. 

The theme stores templates in the directory named after the name of the theme. 

```console
.
└── content
└── themes
    └── codurance
        └── layouts
```

The `layouts` directory contains the template files which are used to generate the site pages based on the content files.

The layout can have different templates to display specific elements. The templates should be stored in the right directories to allow Hugo to find the right template.
Hugo have different types of templates:

- **The single content** - the template used to generate the page for a single content file.

- **The list of content** - the template which is designed to display lists of available content files. It doesn't have the corresponding Markdown file.

- ***index.html*** - the special template for the main page of your site.

Each template have access to variables predefined by Hugo. These variables contains information about the the available content and metadata defined by Hugo. 
Each template type can have access to the information about the whole site via template's variables.  

### The single content template 

To define the way of displaying the data for a single content file you need to define a template for a given **type** and **layout**.
**Type** and **layout** are defined in **the front matter** of the content file.

```
.
└── themes
    └── codurance
        └── layouts
            └── [type]
                └── [layout].html
```

### The list of content

The list of content can be generated for each section. This type of templates allows to display only combined information about the content available for a whole section.
This is the reason why it doesn't have any corresponding Markdown file.

```
.
└── themes
    └── codurance
        └── layouts
            └── section
                └── [section].html
```

By default URL for the list of content is set to `http://codurance.com\[section]\` and there is no possibility to change this address.

### Taxonomies

Hugo allows to group your content into structures named taxonomies. Taxonomies classify the content into groups which demonstrate logical relationships between different pages.
The best example of the taxonomy are `tags`. You can assign as set of tags (groups) to different content files (even from different sections).
The default taxonomies for Hugo are `tags` and `categories`. In Hugo you can also define your own taxonomies in the site config file (see [here](http://gohugo.io/taxonomies/usage/)).

When you want to assign the content to a specific taxonomy you have to define it in **the front matter** of the content file.
Each taxonomy can contain multiple groups and each content can be assigned to multiple groups.

**Example**

```markdown
---
title: (...)
tags: ["my_tag", "another_tag"]
```

To define the way of displaying content which belongs to a group, the right templates must be available in the `taxonomy` directory.
For each taxonomy it must exist a template file with the name of this taxonomy. These templates behave as a list of content. 

```
.
└── themes
    └── codurance
        └── layouts
            └── taxonomy
                └── [taxonomy_name].html
```

This kind of templates generate URL for taxonomies which is in the format `http://codurance.com/[taxonomy_name]/[group]`. 
For example for the `my_tag` group in the `tags` taxonomy Hugo will generate address `http://codurance.com/tags/my_tag`.

# Shortcodes

[TODO]

# Tips and tricks

[TODO]