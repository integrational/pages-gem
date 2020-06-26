- GitHub's original repo:   <https://github.com/github/pages-gem>
- GitHub's original README: <https://github.com/github/pages-gem/blob/master/README.md> (which is misleading)

## How to locally serve/preview a GitHub Pages site using this Docker image

From the local clone of the repo that uses GitHub Pages, run
```
docker run --name gh-pages --rm -it       \
  -p 4000:4000                            \
  -v $(pwd):/src/site                     \
  integrational/gh-pages                  \
  bundle update && bundle exec jekyll serve
```
then browse to <http://127.0.0.1:4000>.
