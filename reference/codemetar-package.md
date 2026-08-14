# codemetar: generate codemeta metadata for R packages

The 'Codemeta' Project defines a 'JSON-LD' format for describing
software metadata, as detailed at <https://codemeta.github.io>. This
package provides utilities to generate, parse, and modify
'codemeta.json' files automatically for R packages, as well as tools and
examples for working with 'codemeta.json' 'JSON-LD' more generally.

## Details

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

## See also

Useful links:

- <https://github.com/ropensci/codemetar>

- <https://docs.ropensci.org/codemetar/>

- Report bugs at <https://github.com/ropensci/codemetar/issues>

## Author

**Maintainer**: Carl Boettiger <cboettig@gmail.com>
([ORCID](https://orcid.org/0000-0002-1642-628X)) \[copyright holder\]

Authors:

- Maëlle Salmon ([ORCID](https://orcid.org/0000-0002-2815-0399))
  \[contributor\]

Other contributors:

- Anna Krystalli ([ORCID](https://orcid.org/0000-0002-2378-4915))
  \[reviewer, contributor\]

- Toph Allen ([ORCID](https://orcid.org/0000-0003-4580-091X))
  \[reviewer\]

- rOpenSci (019jywm96) \[funder\]

- Katrin Leinweber ([ORCID](https://orcid.org/0000-0001-5135-5758))
  \[contributor\]

- Noam Ross ([ORCID](https://orcid.org/0000-0002-2136-0000))
  \[contributor\]

- Arfon Smith \[contributor\]

- Jeroen Ooms ([ORCID](https://orcid.org/0000-0002-4035-0289))
  \[contributor\]

- Sebastian Meyer ([ORCID](https://orcid.org/0000-0002-1791-9449))
  \[contributor\]

- Michael Rustler ([ORCID](https://orcid.org/0000-0003-0647-7726))
  \[contributor\]

- Hauke Sonnenberg ([ORCID](https://orcid.org/0000-0001-9134-2871))
  \[contributor\]

- Sebastian Kreutzer ([ORCID](https://orcid.org/0000-0002-0734-2199))
  \[contributor\]

- Thierry Onkelinx ([ORCID](https://orcid.org/0000-0001-8804-4216))
  \[contributor\]
