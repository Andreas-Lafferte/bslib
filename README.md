
<!-- badges: start -->

[![CRAN
status](https://www.r-pkg.org/badges/version/bootstraplib)](https://cran.r-project.org/package=bootstraplib)
[![Lifecycle:
experimental](https://img.shields.io/badge/lifecycle-experimental-orange.svg)](https://www.tidyverse.org/lifecycle/#experimental)
[![R build
status](https://github.com/rstudio/bootstraplib/workflows/R-CMD-check/badge.svg)](https://github.com/rstudio/bootstraplib/actions)
<!-- badges: end -->

# bootstraplib

The **bootstraplib** R package provides tools for creating custom
[Bootstrap
themes](https://getbootstrap.com/docs/4.4/getting-started/theming/),
making it easier to style Shiny apps and R Markdown documents directly
from R (via **sass**) without writing unruly CSS and HTML. Currently,
**bootstraplib** supports Bootstrap 3 and 4, as well as a special
`"4+3"` compatibility version, which allows you to start using Bootstrap
4 today in Shiny and R Markdown.

## Installation

**bootstraplib** isn’t yet available from CRAN, but you can install
with:

``` r
remotes::install_github("rstudio/bootstraplib")
```

## Getting Started

### Create a theme

Use `bs_theme()` to create a **bootstraplib** theme, where you can:

  - Choose a (major) Bootstrap version.
      - To current `version_default()` is 4+3, which means Bootstrap 4
        plus an additional compatibility layer for Bootstrap 3 style
        navs, navbars, and more. This compatibility allows most Shiny
        apps and R Markdown documents to seamlessly upgrade to Bootstrap
        4.
  - Choose a Bootswatch theme (optional).
  - Customize the main colors and fonts (see example below).
  - More generally, customize any of Bootstrap’s styling defaults via
    Sass variables.

For example, to implement a [material design inspired dark
mode](https://material.io/design/color/dark-theme.html), set the main
colors (and fonts):

``` r
library(bootstraplib)
my_theme <- bs_theme(
  bg = "#202123", fg = "#B8BCC2", primary = "#EA80FC", 
  base_font = "Grandstander"
)
```

### Shiny usage

> Note: this usage requires the development version of Shiny
> `remotes::install_github("rstudio/shiny")`.

To use `my_theme` inside of Shiny, pass it to the relevant `theme`
parameter in page functions such as `shiny::navbarPage()`,
`shiny::fluidPage()`, `shiny::bootstrapPage()`, etc.

``` r
library(shiny)
ui <- navbarPage(
  theme = my_theme,
  ...
)
shinyApp(ui, function(ui, server) {})
```

For a preview of how `my_theme` impacts most of Shiny UI’s styling
defaults, provide it to `bs_theme_preview()`. This preview app includes
most “core” Shiny UI functionality as well as an interactive “real-time”
theming widget for quickly testing out new colors and fonts. To help
replicate those styling changes, the widgets also emits code to the R
console. It can also be used with other Shiny apps via
`run_with_themer()` (or `bs_themer()`).

``` r
bs_theme_preview(my_theme)
```

<img src="https://i.imgur.com/KLKy1s0.gif" style="display: block; margin: auto;" />

More generally, you can use a Bootstrap theme with any HTML page by
using `shiny::bootstrapLib()` to provide the theme as an
`htmltools::htmlDependency()` to an `shiny::htmlTemplate()` or any HTML
`htmltools::tags` that you wish, for example:

``` r
library(shiny)
ui <- htmlTemplate(
  "my-template.html",
  theme = my_theme,
  ...
)
shinyApp(ui, function(ui, server) {})
```

``` html
<!-- my-template.html -->
<!DOCTYPE html>
<html>
  <head>
    {{ headContent() }}
    {{ bootstrapLib(theme) }}
  </head>
  <body>
    ...
  </body>
</html>
```

### R Markdown usage

> Note: this usage currently requires an experimental version of R
> Markdown `remotes::install_github("rstudio/rmarkdown#1706")`

To use a `bs_theme()` in R Markdown, pass the relevant theming
parameter(s) to the `theme` parameter of `html_document` (or, really,
anything that runs through `html_document_base`):

``` yaml
---
output:
  html_document:
    theme:
      bg: "#202123"
      fg: "#B8BCC2"
      primary: "#EA80FC"
      base_font: "Grandstander"
---
```

For backwards-compatibility reasons, R Markdown only uses
**bootstraplib** when `theme` is a list of parameters, so if you want to
just use Bootstrap 4 without any custom theming, you must do:

``` yaml
---
output:
  html_document:
    theme:
      version: 4+3
---
```

Moreover, when `theme` defines a `bs_theme()`, you may modify the
(global) `theme` using functions such as `bs_global_theme_update()` in
**knitr** code chunks (these modifications influence the final Bootstrap
CSS bundle included in the output document).

    ```{r}
    library(bootstraplib)
    bs_global_theme_update(primary = 'green')
    ```

## Learn more

See the articles on [theming
recipes](https://rstudio.github.io/bootstraplib/articles/recipes.html)
and
[foundations](https://rstudio.github.io/bootstraplib/articles/foundations.html).
