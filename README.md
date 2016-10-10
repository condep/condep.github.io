The condep.io Web Site
======================

This GitHub Pages using Jekyll for the ConDep web site located at [http://www.condep.io/](http://www.condep.io/)

Documentation
--------------------

### Adding a new documentation page

To add a new documentation page, you need to:

1) Add a `.md` (markdown) file to _docs/[version]/ with the following header (replace [...]):
```
---
layout: doc-[version]       # example: doc-4-0
title: [page title]         # example: Runbooks
prev_section: [prev-page]   # example: environment
next_section: [next-page]   # example: condep-exe
permalink: [url]            # example: /docs/4-0/artifacts/
---
```
2) Add the page to the navigation menu in `_data/docs-[version].yml`

News
-----------
TBD
