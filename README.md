# kallipos version of pibook

The workflow (source files, pandoc filters) for kallipos
brings together several submodules and builds the content of
the markdown version of the book.

Files are edited at their respective repos and then imported here:

`git submodule update --remote --merge`

The source files are read from the `text` folder,
which is a git submodule, ie a link to another repo.

There is a bash script `make_latex.sh` which can be modified
in order to prepare the desired output format.

## Source files preprocessing

The source files in the `text` folder are formated with 
custom tags for figures, epigraphs, and file inclusion: 
* The include filter needs to be called again if there are nested includes.
* The figure filter should be called after everything has been included.
* The custom tags are translated with lua filters to the desired output format (e.g., latex, html, docx), with [pandoc](https://pandoc.org).
* Remove filters enable to clear unwanted tags and features.

```
pandoc --lua-filter=extras.lua text/mi-ch02.md --to markdown | pandoc --lua-filter=extras.lua --to markdown | pandoc --lua-filter=epigraph.lua --to markdown | pandoc --lua-filter=figure.lua --to markdown | less
```

Some of the include files provide cross-references to figures
and to bibliography and should be processed last.

```
pandoc --filter pandoc-fignos text/mi-ch02.md --to markdown | pandoc --citeproc --bibliography=biblio.bib --to latex | less
```

