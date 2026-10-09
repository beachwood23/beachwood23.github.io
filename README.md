My Blog
===

All posts are copyright Ryan White. Feel free to look at anything else and use my basic blog format. 

---
## Setup

1. Install `ruby-install` (if not already installed):
   ```bash
   brew install ruby-install
   ```

2. Install the Ruby version specified in `.ruby-version`:
   ```bash
   ruby-install ruby $(cat .ruby-version)
   ```

3. Install dependencies:
   ```bash
   bundle install
   ```

---
### Running the blog

Using [Jekyll with Bundler](https://jekyllrb.com/tutorials/using-jekyll-with-bundler/) in order to save plugins and stuff. This uses the Gemfile.

`bundle exec jekyll serve` in the root directory to start a dev server of the blog contents

`bundle exec jekyll build` to build the whole site as static files, and put into `_site`.

