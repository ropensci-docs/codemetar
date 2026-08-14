# create_codemeta

create a codemeta list object in R for further manipulation. Similar to
[`write_codemeta()`](https://docs.ropensci.org/codemetar/reference/write_codemeta.md),
but returns an R list object rather than writing directly to a file. See
examples.

## Usage

``` r
create_codemeta(
  pkg = ".",
  root = ".",
  id = NULL,
  use_filesize = FALSE,
  force_update = getOption("codemeta_force_update", TRUE),
  verbose = TRUE,
  ...
)
```

## Arguments

- pkg:

  package path to package root, or description file (character), or a
  codemeta object (list)

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

- verbose:

  Whether to print messages indicating opinions e.g. when DESCRIPTION
  has no URL. – See
  [`give_opinions`](https://docs.ropensci.org/codemetar/reference/give_opinions.md);
  and indicating the progress of internet downloads.

- ...:

  additional arguments to
  [`write_json`](https://jeroen.r-universe.dev/jsonlite/reference/read_json.html)

## Value

a codemeta list object

## Examples

``` r
# \donttest{
path <- system.file("", package="codemeta")
cm <- create_codemeta(path)
#> … Getting CRAN metadata from RStudio CRAN mirror
#> ✔ Got CRAN metadata!
#> Some elements could be improved, see our opinions via give_opinions('/github/home/R/x86_64-pc-linux-gnu-library/4.6/codemeta/')
#> … Asking README URL from GitHub API
#> ✔ Got README URL!
#> … Asking README URL from GitHub API
#> ✔ Got README URL!
#> … Getting repo topics from GitHub API
#> ✔ Got repo topics!
cm$keywords <- list("metadata", "ropensci")
# }
```
