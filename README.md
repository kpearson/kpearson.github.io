# [Trial and Error](http://trialanderror.io)

A developer blog by Kit Pearson

## Build Notes

To modify the site's JavaScript files I setup a Grunt build script to lint/concatenate/minify all scripts into `scripts.min.js`. Install Node.js, then install Grunt, and then finally install the dependencies for the theme contained in `package.json`:

```bash
npm install {% endhighlight %}
```

From the theme's root, use grunt concatenate JavaScript files, and optimize .jpg, .png, and .svg files in the images/ folder. You can also use grunt dev in combination with jekyll build --watch to watch for updates JS files that Grunt will then automatically re-build as you write your code which will in turn auto-generate your Jekyll site when developing locally.

## Acknowledgements

I'm very greatfull to [Michael Rose](https://mademistakes.com/) for the
**[Minimal Mistakes](http://mmistakes.github.io/minimal-mistakes)** Jekyll
theme. It is the foundation of my Jekyll blog and the source of many learnings.
Hats off. :-)

Github I love your pages. Thank you.
