# Overview

Tools for the web site https://swib.org


## Prerequisites on Ubuntu 22.04
To be able to process some packages have to be installed and the `MAKEFILE` has to be adjusted:

Perl:
```
# apt install libtypes-datetime-perl; apt install libhtml-template-perl; apt install libpath-tiny-perl; apt install libxml-libxml-perl; apt install libyaml-tiny-perl
```
Pandoc:
```
# apt install pandoc
```
Adjust pathes for `Makefile`:
```
# ln -s /usr/bin/pandoc  /usr/local/bin/pandoc
```
Adjust `TEMPLATE_DIR` to ` ~/git/swib.org/etc/pandoc_templates/`

## Process

* `get_conftool_data.sh` fetches the source data from the SWIB Conftool website.

* `create_website.pl` creates markdown files for each dynamic page and one turtle file

* `make` invokes Pandoc for converting .md to .html files

* `sync_to_live.sh` transfers the pages to the live website

* `recreate_charts.sh` creates participant statistics and charts for internal use

## Configuration

Config variables are supplied by `config.yaml` - in particular the name of the
current SWIB edition, which is used by all procedures.

## Security / privacy

Password files (eg, .conftool_secret) and conftool source files, which contain
personal data, are not added to Git (see `/.gitignore`).

## Templates

`create_website.pl` creates the dynamic pages (programme, speakers), using
HTML::Template templates in `./etc/html_tmpl`. These pages include
sidebar.md.inc, which in turn includes newsbox.md.inc.

Another level of template is applied for all (dynamic and static) pages, namely
the `etc/pandoc_tmpl/default.html5` template for the html output in the Pandoc
convertsion. This template is used (in combination with a Pandoc variable set
in the `Makefile` to create the navigation menu, with an indication of the
current page.

Include files in Pandoc (partials), as they would be useful for the sidebar,
work from v2.9 on. Unfortunately, CentOS 7 has only v2.7.3, and Ubuntu 20-04
has only v2.5. So with the software versions at ZBW, it is impossible to use
one sidebar include file for static and dynamic pages. So all pages are
generated via HTML::Template.

**This means that the regular pages (linked in the menu) should not be changed
in their html/\*.md version, but only in `./etc/html_tmpl/*.md.tmpl`!!**

## Connection between contributions and speakers

For creating the author links from the programme page to their speaker entry,
the author email addresses in the "Presentation" are essential. Currently
(SWIB22), for the the "Submitted by" form fields of the "Contributors
Biography" are used.

### TODO

Currently, this creates some confusion and additional work, since the
internally saved "Sumitting Author" (by user ID)  is not necessarily identical
with the form fields "Submitted by". Additionally, when people add their
co-authors, they are often not aware that they should supply the co-authors
details in "Submitted by". Check for 2023, if other form settings and use of 
Conftool variables could improve the situation.

## Empty author and abstract fields

Empty entries for abstracts (e.g., for "Closing") and authors (e.g., for
"Lighning talks") are not allowed by Contftool. So a '.' in the abstract
indicates that abstract output should be omitted, and '.' in the first
firstname and lastname Author entries indicate, that author and organisation
should be omitted for the contribution.

