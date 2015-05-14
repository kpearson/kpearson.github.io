---
layout: post
title: Jekyll guide
---

Does not seem to be currently possible server side on any GitHub Pages supported Markdown engine.

Introduction

Jekyll is a static site generator that can use many markup interpreting engines, in particular kramdown, which is one of the best.

The generated site is put under _site directory, which should be ignored.

The Liquid template engine is used in Jekyll.

What Jekyll does is to add many variables automatically to the templates and then possibly compile the result via some markdown format to make Liquid into a blog / website.

The main force behind Jekyll is that GitHub Pages hosts it for free.

Liquid

http://liquidmarkup.org/

Good wiki docs: https://github.com/Shopify/liquid/wiki/Liquid-for-Designers

Meant to be client facing safe and fast, and therefore limited by design for sandboxing.

Liquid was extracted from Shopify.

Variable

assign

Set a variable to a string:

var = val

Int:

var = 1

Filter

Filters are Jekyll’s cumbersome implementation of functions.

TODO: possible to apply a filter and take an attribute without assigning to a variable first?

{“a”=>”a0”, “b”=>”b0”}

List

List literals

List: seems not to have literals.

Workarounds:

YAML front matter:

a0
b0

a1
b1

a

b
split:

var[0] = a
var[1] = b
Fails:

var[0] = 
var[1] = 
first list filter

{“a”=>”a0”, “b”=>”b0”}

Hash

Map

Hash literal

TODO seems impossible except for frontmatter?

Filter array of hashes

where

b1

Markup

Markup is decided based on file extension.

You must use the triple slash metadata header for markdown to be interpreted on the index file.

The default markdown engine was Maruku, but the project was discontinued. Kramdown is recommended as replacement for Maruku.

The default won’t be changing too soon for backwards compatibility: https://github.com/jekyll/jekyll/issues/126#issue/126/comment/125723. Watch out in particular for the colon : on first line bug of Maruku.

Pandoc will not be making it to GitHub Pages anytime soon: https://github.com/jekyll/jekyll/issues/1973

Pages

Page 0 Page 1 dir/Page 0 submodule/

page.url = /index.html

page.url = /CASE.html
page.title =
page.permalink =
page.path = CASE.md
page.url = /case.html
page.title =
page.permalink =
page.path = case.md
page.url = /submodule/index.html
page.title = Submodule
page.permalink =
page.path = submodule/index.md
page.url = /index.html
page.title = index title
page.permalink =
page.path = index.md
page.url = /page-no-perma.html
page.title = Page No Perma
page.permalink =
page.path = page-no-perma.md
page.url = /d/page-perma-subdir/
page.title = Page Perma Subdir
page.permalink = d/page-perma-subdir/
page.path = page-perma-subdir.md
page.url = /noindex/page0/
page.title = Page 0 title
page.permalink = page0/
page.path = noindex/page0.md
page.url = /dir/page0/
page.title = dir/Page 0 title
page.permalink = page0/
page.path = dir/page0.md
page.url = /page0/
page.title = Page 0 title
page.permalink = page0/
page.path = page0.md
page.url = /page1/
page.title = Page 1 title
page.permalink = page1/
page.path = page1.md
Site

site.pages contains an array of all page hashes for the site.

Link to a page with it’s title:

Posts

TODO make post URL work on GitHub pages (not using the site prefix)

post.title = post1 title

post.url = /post1

post.date = 2000-01-02 00:00:00 +0000

post.categories = category0 and category02

post.tags = tag0 and tag2

post.excerpt = { post.excerpt }

post.content = { post.content }

post.title = post0 title

post.url = /post0

post.date = 2000-01-01 00:00:00 +0000

post.categories = category0 and category01

post.tags = tag0 and tag1

post.excerpt = { post.excerpt }

post.content = { post.content }

site.categories.category0 = <p>Post 1 content.</p> and <p>Post 0 content.</p>

Inline x2 math.

page.date | date: "%Y-%M-%d" = 2000-00-01

site.categories.tag0 = <p>Post 1 content.</p> and <p>Post 0 content.</p>

Inline x2 math.

page.date | date: "%Y-%M-%d" = 2000-00-01

Layout

There is no current way to specify a default layout, but there is a PR on its way: https://github.com/jekyll/jekyll/pull/1527

Data

Works like an YAML text database.

entry.key0 = val0
entry.key1 = val0

entry.key0 = val1
entry.key1 = val1
Data in _config.yml (not reparsed by --watch, must rebuild):

site.custom-key0 = site.custom-key0

site.custom-key1 = site.custom-key1

Image

Kramdown:

image

Tags added by Jekyll

There are extra tags added by Jekyll to Liquid.

Code

To have syntax highlighting, you need the corresponding CSS file included: the highlighter only adds classes.

def f
    1
end
1 def f
2     1
3 end
Kramdown fenced code block TODO: how to syntax highlight (HTML classes not being added).

def f(x)
  x + 1
end
Gist

gist 8749681 =

1
2
3
def f0(x)
  x + 0
end
view raw0.rb hosted with ❤ by GitHub
1
2
3
def f1(x)
  x + 1
end
view raw1.rb hosted with ❤ by GitHub
gist 8749681 0 =

1
2
3
def f0(x)
  x + 0
end
view raw0.rb hosted with ❤ by GitHub
Include

include includes0.md key="val0" =

Includes 0 content is markdown.

include.key: val0

include includes1.md key="val0" =

Includes 1 content is markdown.

include.key: val1

post_url

post_url 2000-01-01-post0 = /post0

Link to post with it’s title shown

TODO better way?

Page 0 title

Filters added by Jekyll

date_to_string

site.time = 2014-12-21 09:44:02 +0000

site.time | date_to_string = 21 Dec 2014

Math

The best possibility without manual pre push pre processing seems to be to:

markdown: kramdown on the _config.yml
MathJax JavaScript on the header
To our knowledge there is no server side option (MathML or images) that will run on GitHub Pages without local precompiling + adding output files to the Git repo.

Inline x2 math.

Block:

x2
x2
https://github.com/jekyll/jekyll/issues/1888 seems solved:

mn=|r|=|h|
Symlinks

Pages only build it:

in .nojekyls mode

the symlink points into the repository: https://help.github.com/articles/page-build-failed-symlink-does-not-exist-within-your-site-s-repository/

The following generate a build error:

..
..\0
The following don’t generate a build error but show 404:

.git
.git/config
not-a-file
The following work:

Gemfile
images
../images/Gemfile
nojekyll

To turn off Jekyll entirely, add a .nojekyll file to the top-level: https://help.github.com/articles/using-jekyll-with-pages/#turning-jekyll-off
