- GitHub's original repo:   <https://github.com/github/pages-gem>
- GitHub's original README: <https://github.com/github/pages-gem/blob/master/README.md> (which is misleading)

## How to locally create the content of a GitHub Pages site

From the currently empty local clone of a repo that is to be populated with the content of a GitHub Pages site, run
```bash
docker run --name gh-pages --rm -it \
  --workdir /src/gh/pages-gem       \
  -v $(pwd):/src/site               \
  integrational/gh-pages            \
  bundle exec jekyll new /src/site
```
then strip-down `Gemfile` to
```ruby
source "https://rubygems.org"
gem "github-pages", group: :jekyll_plugins
```
and refine `_config.yml` as follows:

- remove `description` because it's taken from the GitHub repo by default
- set `baseurl: "/repo"` if `repo` is the name of the GitHub repo

The content of this directory can now be pushed to GitHub and served/previewed locally as described next.

## How to locally serve/preview a GitHub Pages site

From the local clone of a repo that uses GitHub Pages, run
```bash
docker run --name gh-pages --rm -it                    \
  -p 4000:4000                                         \
  -v $(pwd):/src/site                                  \
  integrational/gh-pages                               \
  bundle update && bundle exec jekyll serve --baseurl ''
```
then browse to <http://127.0.0.1:4000>.

## How to set relative links in the content of a GitHub Pages site

The above instructions combine the setting of `baseurl` in `_config.yml` with overriding it with `--baseurl ''` when serving locally.

[This post](https://github.com/jekyll/jekyll/issues/332#issuecomment-18952908) explains this approach and specifies how to construct links in the site content. In short:

- `{{ site.baseurl }}/path/to/css.css`
- `{{ site.baseurl }}{{ post.url }}`
