---
paths:
  - "**/*.{R,r,Rmd,qmd}"
  - "**/DESCRIPTION"
---

# R

A project `CLAUDE.md` or `AGENTS.md` overrides this file. The r-skills plugin covers general style, tidyverse, rlang, and package development. These rules add to it or override it.

## Code style

- Do not use `return()` at the end of a function, even a long one. Use `return()` only for an early exit.
- Use four dashes for section headings: `# Load data ----`.
- Do not use partial argument matching.
- A function with side effects returns its first argument invisibly.
- In scripts, call functions from loaded packages directly. Use `pkg::fn()` only for one or two calls, for a name conflict, or in package code.

## Tools

- After you write or change a `.R` file, format it. If the project uses a formatter, use that formatter. If not, use Air (`air format <path>`). Air does not format code chunks in R Markdown or Quarto files.
- Use `cli` for console output (for example, `cli::cli_alert_success()`). Do not use `cat()`, `message()`, or `print()`.
- For data that is not clean, use `janitor::clean_names()`.
- For figures, maps, and tables, use the viz skill.

## Project stack

For a complex project, use these tools:

| Need | Tool |
|------|---------|
| Dependencies | `renv` |
| Paths and settings | `here`, `config` |
| Data versions | `pins` |
| Modules and pipeline | `box`, `targets` |
| Logs and profiling | `logger`, `profvis` |
| UI and server | `shiny`, `shinyreact` |
| Reports | Quarto |

- For a UI and server, use `shiny` with `bslib` for dashboards and fast prototypes. Use [`shinyreact`](https://posit-dev.github.io/shinyreact/) when the UI needs custom React components or client-side interaction without a server round trip. `shinyreact` is pre-release.
- In a `shinyreact` app, the server sends only data with `reactive_output()`. React renders all of the UI from `ui.tsx`, and `page_react()` serves it. Do not build UI in the server.
- For reports, use Quarto. For a PDF, use `format: typst`, not `format: pdf` (LaTeX). Use LaTeX only when a required template needs it.
- Put the logic in functions in `R/`. Scripts and the pipeline only call these functions.
- Use this layout: `R/` for functions, `_targets.R` for the pipeline, `config.yml` for settings, and `data-dict.yaml` for the data contract.
- `box::use()` keeps a module in cache until the R session stops. After you change a module file, restart R or call `box::reload()`. Then do the tests.

## Package development

- Keep the error context: use `withCallingHandlers()` with `parent = e`, or `rlang::try_fetch()`. Do not throw an error again inside `tryCatch()`.
- Use `rlang::check_installed()` for suggested dependencies.
- Use `@importFrom` only for operators (for example, `%||%`), frequent calls, and tight loops. Use `@import` only when necessary.
- Use `pkgdown` with `light-switch` set to `true`.
