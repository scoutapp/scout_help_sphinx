# Scout Help Docs

This is the documentation site for [Scout](https://scoutapp.com), powered by [Sphinx](http://www.sphinx-doc.org/) and [ReadTheDocs](https://readthedocs.org/). 

## Development

* Clone the repo
* `pip install -r requirements.txt`
* `cd docs`
* `rm -Rf _build/*;sphinx-autobuild . _build/html`

## Builds

A fresh build of the docs is automatically triggered via a Git webhook to readthedocs.org. This makes simple changes easy - just use the GitHub web UI to make the change.

## Styleguide

* `rst` files are used for index pages to render a table of contents. Everything else uses Markdown as that is what we are most familiar with. `rst` formatting should be kept to a minimum (don't want to learn another formatting syntax).
* For content that is likely to be heavily searched via search engines, create a dedicated page. 
* Page titles should be `<h1>` tags.

## Known Quirks

* I've occasionally seen the build directory not refresh with changes, especially those related to TOC blocks. `rm -Rf _build/` and restarting seems to fix.
