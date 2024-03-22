# sphinx-epub-issue

I found this issue when generating an epub document using [Sphinx](https://www.sphinx-doc.org/en/master/index.html) and the Spinx extension [opengraph](https://github.com/wpilibsuite/sphinxext-opengraph).

The opengraph extension can generate a description based on the beginning text on the page. If this text happens to have something resembling an HTML tag (enclosed with "<" and ">"), an error can be found when validating the xhtml used when generating an [EPUB](https://www.w3.org/publishing/epub3/) document.

[PR #120](https://github.com/wpilibsuite/sphinxext-opengraph/pull/120) was created to address this issue by cleaning up the HTML that gets inserted into the description.

## Setup the environment and build the docs
```
python3 -m venv .env
. .env/bin/activate
pip install -r dev-requirements.txt
make html epub
```

## Check the xhtml
Running a check on the xhtml produces this error:
```shell
$ htmlcheck page _build/epub/index.xhtml
10:39:50 - Launching validation for 1 paths
10:39:51 - .../sphinx-epub-issue/_build/epub/index.xhtml
10:39:51 - From line 11 column 1 to line 11 column 60
10:39:51 - attribute values may not contain '<'
10:39:51 - issue" />\n<meta property="og:description" content="This is a test of <tags>
10:39:51 - From line 1 column 16 to line 3 column 85
10:39:51 - Consider adding a “lang” attribute to the “html” start tag to declare the language of this document.
10:39:51 - TYPE html>\n\n<html xmlns="http://www.w3.org/1999/xhtml" xmlns:epub="http://www.idpf.org/2007/ops">\n  <he
```

And, after importing the generated EPUB into the MacOS Books app, this error is displayed:
```
“This page contains the following errors:

error on line 11 at column 60: Unescaped '<' not allowed in attributes values

Below is a rendering of the page up to the first error.”
```
