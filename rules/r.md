---
paths:
  - "**/*.{R,r,Rmd,qmd}"
  - "**/DESCRIPTION"
---

# R

The r-skills plugin covers general style, tidyverse, rlang, and package development. These rules add to it or override it.

- Do not use `return()` at the end of a function, even a long one. Use `return()` only for an early exit.
- Use four dashes for section headings: `# Load data ----`.
- Do not use partial argument matching.
- After you write or change a `.R` file, format it. If the project uses a formatter (for example, `air.toml`, or `styler` in a pre-commit hook or CI), use that formatter. If not, use Air (`air format <path>`), not `styler`. Air does not format code chunks in R Markdown or Quarto files.
- Use `cli` for console output (for example, `cli::cli_alert_success()`). Do not use `cat()`, `message()`, or `print()`.
- A function with side effects returns its first argument invisibly.
- For data that is not clean, use `janitor::clean_names()`.
- In scripts, call functions from loaded packages directly. Use `pkg::fn()` only for one or two calls, for a name conflict, or in package code.
- For complex projects, these packages can help: `renv`, `config`, `here`, `pins`, `box`, `targets`, `logger`, and `profvis`.
- `box::use()` keeps a module in cache until the R session stops. After you change a module file, restart R or call `box::reload()`. Then do the tests.

## Package Development

- Keep the error context: use `withCallingHandlers()` with `parent = e`, or `rlang::try_fetch()`. Do not throw an error again inside `tryCatch()`.
- Use `rlang::check_installed()` for suggested dependencies.
- Use `@importFrom` only for operators (for example, `%||%`), frequent calls, and tight loops. Use `@import` only when necessary.
- Use `pkgdown` with `light-switch` set to `true`.

## Figures and tables

These rules follow [Beyond Bar and Box Plots](https://z3tt.github.io/beyond-bar-and-box-plots). The research-writing skill gives the general figure rules.

### Distributions

- For a small sample, show each point with `ggbeeswarm::geom_quasirandom()` or `ggforce::geom_sina()`. Do not show a violin or a density alone, because its shape is not reliable for few points.
- For a large sample, show the shape with `ggdist` (`stat_halfeye()`, `stat_interval()`, or `geom_dots()`) or `ggridges`. Add the data points when they stay readable.
- For a raincloud plot, put `ggdist::stat_halfeye()` on one side, a narrow `geom_boxplot()` in the middle, and the points on the other side with `ggdist::stat_dots(side = "left")`. Use `justification` to move the ggdist layers away from the box plot.
- Do not use a box plot alone, because it hides a distribution with more than one peak. Put the data points on it.
- Keep the default density bandwidth. If you change `adjust`, tell the value in the caption.

### Layers and labels

- Put summary layers below the points. Use `alpha` so that one layer does not hide another. With `geom_boxplot()` and points, set `outlier.shape = NA` so that each outlier shows only one time.
- For jitter, set a small `width` and `height = 0`, so that the values on the value axis stay correct.
- Put the sample size in each group label, for example `"{group}\n(n = {n})"`.
- Order the groups by a value that has a meaning, for example `forcats::fct_reorder(group, value, median)`. Do not use alphabetical order unless it has a meaning.
- Use a palette that is safe for color blindness, for example `rcartocolor` or `viridis`. Give each group the same color in all figures.

### Models and tables

- For model coefficients, Likert data, and proportions, use `ggstats`.
- For tables, use `gt`.
