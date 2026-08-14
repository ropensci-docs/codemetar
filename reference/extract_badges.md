# Extract all badges from Markdown file

Extract all badges from Markdown file

## Usage

``` r
extract_badges(path)
```

## Arguments

- path:

  Path to Markdown file

## Value

A data.frame with for each badge its text, link and link to its image.

## Examples

``` r
if (FALSE) { # \dontrun{
extract_badges(system.file("examples/README_fakepackage.md", package="codemetar"))
} # }
```
