# tinkidinki.github.io

Welcome to the repository of my personal website, where you can see behind the scenes of my website!

The website uses the [minima](https://github.com/jekyll/minima) theme, as well as the stylesheets from the [personal-website](https://github.com/github/personal-website) repository.

In order to redirect some pages to external sites, for example, the `projects` page, I used the [jekyll-redirect-from gem](https://github.com/jekyll/jekyll-redirect-from). 

To make mathjax work, I followed the instructions from this [site](http://webdocs.cs.ualberta.ca/~zichen2/blog/coding/setup/2019/02/17/how-to-add-mathjax-support-to-jekyll.html). However, while this rendered locally, this did not render on `https` pages. For mathjax to work on `https`, I had to modify the line in `head.html` slightly, as discussed in [this comment](https://github.com/github/pages-gem/issues/307#issuecomment-275747524).
