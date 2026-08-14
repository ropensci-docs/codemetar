# Intro to codemetar, Codemeta creator for R packages

## codemetar: generate codemeta metadata for R packages

**Why codemetar?** The ‘Codemeta’ Project defines a ‘JSON-LD’ format for
describing software metadata, as detailed at
<https://codemeta.github.io>. This package provides utilities to
**generate, parse, and modify codemeta.jsonld files automatically for R
packages**, as well as tools and examples for **working with codemeta
json-ld more generally**.

It has three main goals:

- Quickly **generate a valid codemeta.json file from any valid R
  package**. To do so, we automatically extract as much metadata as
  possible using the DESCRIPTION file, as well as extracting metadata
  from other common best-practices such as the presence of Travis and
  other badges in README, etc.
- Facilitate the addition of further metadata fields into a
  codemeta.json file, as well as general manipulation of codemeta files.
- Support the ability to crosswalk between terms used in other metadata
  standards, as identified by the Codemeta Project Community, see
  <https://codemeta.github.io/crosswalk/>

### Why create a codemeta.json for your package?

**Why bother creating a codemeta.json for your package?** R packages
encode lots of metadata in the `DESCRIPTION` file, `README`, and other
places, telling users and developers about the package purpose, authors,
license, dependencies, and other information that facilitates discovery,
adoption, and credit for your software. Unfortunately, because each
software language records this metadata in a different format, that
information is hard for search engines, software repositories, and other
developers to find and integrate.

By generating a `codemeta.json` file, you turn your metadata into a
format that can easily crosswalk between metadata in many other software
languages. CodeMeta is built on [schema.org](https://schema.org) a
simple [structured
data](https://developers.google.com/search/docs/advanced/structured-data/intro-structured-data)
format developed by major search engines like Google and Bing to improve
discoverability in search. CodeMeta is also understood by significant
software archiving efforts such as [Software
Heritage](https://www.softwareheritage.org/) Project, which seeks to
permanently archive all open source software.

For more general information about the CodeMeta Project for defining
software metadata, see <https://codemeta.github.io>. In particular, new
users might want to start with the [User
Guide](https://codemeta.github.io/user-guide/), while those looking to
learn more about JSON-LD and consuming existing codemeta files should
see the [Developer Guide](https://codemeta.github.io/developer-guide/).

### Installation and usage requirements

You can install the latest version from CRAN using:

``` r

install.packages("codemetar")
```

You can also install the development version of `codemetar` from GitHub
with:

``` r

# install.packages("devtools")
devtools::install_github("ropensci/codemetar")
```

For optimal results you need a good internet connection.

The package queries

- [`utils::available.packages()`](https://rdrr.io/r/utils/available.packages.html)
  for CRAN and Bioconductor packages;

- GitHub API via the [`gh` package](https://github.com/r-lib/gh), if it
  finds a GitHub repo URL in DESCRIPTION or as git remote. GitHub API is
  queried to find the [preferred
  README](https://developer.github.com/v3/repos/contents/#get-the-readme),
  and the [repo
  topics](https://developer.github.com/v3/repos/#list-all-topics-for-a-repository).
  If you use codemetar for many packages having a
  [GITHUB_PAT](https://github.com/r-lib/gh#environment-variables) is
  better;

- [R-hub sysreqs API](https://r-hub.github.io/rhub/) to parse
  SystemRequirements.

If your machine is offline, a more minimal codemeta.json will be
created. If your internet connection is poor or there are firewalls, the
codemeta creation might indefinitely hang.

### Create a codemeta.json in one function call

`codemetar` can take the path to the source package root to glean as
much information as possible.

``` r

codemetar::write_codemeta()
```

``` r

library("magrittr")
"../../codemeta.json" %>%
  details::details(summary = "codemetar's codemeta.json",
                   lang = "json")
```

By default most often from within your package folder you’ll simply run
[`codemetar::write_codemeta()`](https://docs.ropensci.org/codemetar/reference/write_codemeta.md).

### Keep codemeta.json up-to-date

**How to keep codemeta.json up-to-date?** In particular, how to keep it
up to date with `DESCRIPTION`? `codemetar` itself no longer supports
automatic sync, but there are quite a few methods available out there.
Choose one that fits well into your workflow!

- You could rely on `devtools::release()` since it will ask you whether
  you updated codemeta.json when such a file exists.

- You could use a git pre-commit hook that prevents a commit from being
  done if DESCRIPTION is newer than codemeta.json.

  - You can use the [precommit
    package](https://github.com/lorenzwalthert/precommit) in which
    there’s a “codemeta-description-updated” hook.

  - If that’s your only pre-commit hook (i.e. you don’t have one created
    by
    e.g. [`usethis::use_readme_rmd()`](https://usethis.r-lib.org/reference/use_readme_rmd.html)),
    then you can create it using

``` r

script = readLines(system.file("templates", "description-codemetajson-pre-commit.sh", package = "codemetar"))
usethis::use_git_hook("pre-commit",
                     script = script)
```

- You could use GitHub actions. Refer to GitHub actions docs
  <https://github.com/features/actions>, and to the example workflow
  provided in this package (type
  `system.file("templates", "codemeta-github-actions.yml", package = "codemetar")`).
  You can use the `cm-skip` keyword in your commit message if you don’t
  want this to run on a specific commit. The example workflow provided
  is setup to only run when a push is made to the master branch. This
  setup is designed for if you’re using a [git
  flow](https://nvie.com/posts/a-successful-git-branching-model/#the-main-branches)
  setup where the master branch is only committed and pushed to via pull
  requests. After each PR merge (and the completion of this GitHub
  action), your master branch will always be up to date and so long as
  you don’t make manual changes to the codemeta.json file, you won’t
  have merge conflicts.

Alternatively, you can have GitHub actions route run `codemetar` on each
commit. If you do this you should try to remember to run `git pull`
before making any new changes on your local project. However, if you
forgot to pull and already committed new changes, fret not, you can use
([`git pull --rebase`](https://stackoverflow.com/questions/18930527/difference-between-git-pull-and-git-pull-rebase/38139843#38139843))
to rewind you local changes on top of the current upstream `HEAD`.

click here to see the workflow

``` yaml

on:
  push:
    branches: master
    paths:
      - DESCRIPTION
      - .github/workflows/main.yml

name: Render codemeta
jobs:
  render:
    name: Render codemeta
    runs-on: macOS-latest
    if: "!contains(github.event.head_commit.message, 'cm-skip')"
    steps:
      - uses: actions/checkout@v1
      - uses: r-lib/actions/setup-r@v1
      - name: Install codemetar
        run: Rscript -e 'install.packages("codemetar")'
      - name: Render codemeta
        run: Rscript -e 'codemetar::write_codemeta()'
      - name: Commit results
        run: |
          git commit codemeta.json -m 'Re-build codemeta.json' || echo "No changes to commit"
          git push https://${{github.actor}}:${{secrets.GITHUB_TOKEN}}@github.com/${{github.repository}}.git HEAD:${{ github.ref }} || echo "No changes to commit"
```

  

### A brief intro to common terms we’ll use:

- [Linked data](https://en.wikipedia.org/wiki/Linked_data): We often use
  different words to mean the same thing. And sometimes the same word to
  mean different things. Linked data seeks to address this issue by
  using
  [URIs](https://en.wikipedia.org/wiki/Uniform_Resource_Identifier)
  (i.e. URLs) to make this explicit.

- context: No one likes typing out long URLs all the time. So instead,
  the *context* of a JSON-LD file (`"@context"` element) gives us the
  context for the terms we use, that is, the root URL. Usually
  schema.org but domain specific ones also (eg codemeta)

- [Schema.org](https://schema.org/): A major initiative led by Google
  and other search engines to define a simple and widely used context to
  link data on the web through a catalogue of standard metadata fields

- [The CodeMeta Project](https://codemeta.github.io/): an academic led
  community initiative to formalise the metadata fields included in
  typical software metadata records and introduce important fields that
  did not have clear equivalents. The codemeta
  [crosswalk](https://codemeta.github.io/crosswalk/) provides an
  explicit map between the metadata fields used by a broad range of
  software repositories, registries and archives

- [JSON-LD](https://json-ld.org): While ‘linked data’ can be represented
  in many different formats, these have consistently proven a bit tricky
  to use, either for consumers or developers or both. JSON-LD provides a
  simple adaptation of the JSON format, which has proven much more
  popular with both audiences, that allows it to express (most)
  linked-data concepts. It is now the format of choice for expressing
  linked data by Google and many others. Any JSON-LD file is valid JSON,
  and any JSON file can be treated as JSON-LD.

- [codemetar](https://codemeta.github.io/codemetar/): The CodeMeta
  Project has created tools in several languages to implement the
  CodeMeta Crosswalk (using JSON-LD) and help extract software metadata
  into `codemeta.json` records. `codemetar` is one such tool, focused on
  R and R packages.

### How to improve your package’s codemeta.json?

The best way to ensure `codemeta.json` is as complete as possible is to
set metadata in all the usual places, and then if needed add more
metadata.

To ensure you have metadata in the usual places, you can run
[`codemetar::give_opinions()`](https://docs.ropensci.org/codemetar/reference/give_opinions.md).

#### Usual terms in DESCRIPTION

- Fill `BugReports` and `URL`.

- Using the `Authors@R` notation allows a much richer specification of
  author roles, correct parsing of given vs family names, and email
  addresses.

In the current implementation, developers may specify an ORCID url for
an author in the optional `comment` field of `Authors@R`, e.g.

    Authors@R: c(person(given = "Carl",
                 family = "Boettiger",
                 role = c("aut", "cre", "cph"),
                 email = "cboettig@gmail.com",
                 comment = c(ORCID = "0000-0002-1642-628X")))

which will allow `codemetar` to associate an identifier with the person.
This is clearly something of a hack since R’s `person` object lacks an
explicit notion of `id`, and may be frowned upon.

#### Usual terms in the README

In the README, you can use badges for continuous integration, repo
development status (repostatus.org or lifecycle.org), provider
([e.g. for CRAN](https://r-hub.github.io/rhub/)).

#### GitHub repo topics

If your package source is hosted on GitHub and there’s a way for
codemetar to determine that (URL in DESCRIPTION, or git remote URL)
codemetar will use [GitHub repo
topics](https://docs.github.com/en/github/administering-a-repository/classifying-your-repository-with-topics)
as keywords in codemeta.json. If you also set keywords in DESCRIPTION
(see next section), codemetar will merge the two lists.

#### Set even more terms via DESCRIPTION

In general, setting metadata via the places stated earlier is the best
solution because that metadata is used by other tools (e.g. the URLs in
DESCRIPTION can help the package users, not only codemetar).

The DESCRIPTION file is the natural place to specify any metadata for an
R package. The `codemetar` package can detect certain additional terms
in the [CodeMeta context](https://codemeta.github.io/terms/). Almost any
additional codemeta field can be added to and read from the DESCRIPTION
into a `codemeta.json` file (see `codemetar:::additional_codemeta_terms`
for a list).

CRAN requires that you prefix any additional such terms to indicate the
use of `schema.org` explicitly, e.g. `keywords` would be specified in a
DESCRIPTION file as:

    X-schema.org-keywords: metadata, codemeta, ropensci, citation, credit, linked-data

Where applicable, these will override values otherwise guessed from the
source repository. Use comma-separated lists to separate multiple values
to a property, e.g. keywords.

See the
[DESCRIPTION](https://github.com/ropensci/codemetar/blob/master/DESCRIPTION)
file of the `codemetar` package for an example.

#### Set the branch that codemetar references

There are a number of places that codemetar will reference a github
branch if your code is hosted on github (e.g. for release notes, readme,
etc.). By default, codemetar will use the name “master” but you can
change that to whatever your default branch is by setting the option
“codemeta_branch” (e.g. `options(codemeta_branch = "main")` before
calling
[`write_codemeta()`](https://docs.ropensci.org/codemetar/reference/write_codemeta.md)
to use the branch named “main” as the default branch).

### Going further

Check out all the [codemetar
vignettes](https://docs.ropensci.org/codemetar/articles/index.html) for
tutorials on other cool stuff you can do with codemeta and json-ld.

A new feature is the creation of a minimal schemaorg.json for insertion
on your website’s webpage for Search Engine Optimization, when the
`write_minimeta` argument of
[`write_codemeta()`](https://docs.ropensci.org/codemetar/reference/write_codemeta.md)
is `TRUE`.

You could e.g. use the code below in a chunk in README.Rmd with
`results="asis"`.

``` r

glue::glue('<script type="application/ld+json">
      {glue::glue_collapse(readLines("schemaorg.json"), sep = "\n")}
    </script>')
```

Refer to [Google
documentation](https://developers.google.com/search/docs) for more
guidance.
