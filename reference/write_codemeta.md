# write_codemeta

write out a codemeta.json file for a given package. This function is
basically a wrapper around create_codemeta() to both create the codemeta
object and write it out to a JSON-LD-formatted file in one command. It
can also be used simply to write out to JSON-LD any existing object
created with
[`create_codemeta()`](https://docs.ropensci.org/codemetar/reference/create_codemeta.md).

## Usage

``` r
write_codemeta(
  pkg = ".",
  path = "codemeta.json",
  root = ".",
  id = NULL,
  use_filesize = TRUE,
  force_update = getOption("codemeta_force_update", TRUE),
  use_git_hook = NULL,
  verbose = TRUE,
  write_minimeta = FALSE,
  ...
)
```

## Arguments

- pkg:

  package path to package root, or description file (character), or a
  codemeta object (list)

- path:

  file name of the output, leave at default "codemeta.json"

- root:

  if pkg is a codemeta object, optionally give the path to package root.
  Default guess is current dir.

- id:

  identifier for the package, e.g. a DOI (or other resolvable URL)

- use_filesize:

  whether to try to estimating and adding a filesize by using
  [`base::file.size()`](https://rdrr.io/r/base/file.info.html). Files in
  `.Rbuildignore` are ignored.

- force_update:

  Update guessed fields even if they are defined in an existing
  codemeta.json file

- use_git_hook:

  Deprecated argument.

- verbose:

  Whether to print messages indicating opinions e.g. when DESCRIPTION
  has no URL. – See
  [`give_opinions`](https://docs.ropensci.org/codemetar/reference/give_opinions.md);
  and indicating the progress of internet downloads.

- write_minimeta:

  whether to also create the file schemaorg.json that corresponds to the
  metadata Google would validate, to be inserted to a webpage for SEO.
  It is saved as "inst/schemaorg.json" alongside `path` (by default,
  "codemeta.json").

- ...:

  additional arguments to
  [`write_json`](https://jeroen.r-universe.dev/jsonlite/reference/read_json.html)

## Value

writes out the codemeta.json file, and schemaorg.json if
`write_codemeta` is `TRUE`.

## Technical details

If pkg is a codemeta object, the function will attempt to update any
fields it can guess (i.e. from the DESCRIPTION file), overwriting any
existing data in that block. In this case, the package root directory
should be the current working directory.

When creating and writing a codemeta.json for the first time, the
function adds "codemeta.json" to .Rbuildignore.

## Examples

``` r
if (FALSE) { # \dontrun{
# from anywhere in the package source directory
write_codemeta()
} # }
```
