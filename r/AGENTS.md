# R

## r installation manager: rig

- installed at the system level
- run: `rig run -r 4.4.3 -f main.R`
- run with args:
	- `rig run -r 4.4.3 -f main.R -- --help`
	- `rig run -r 4.4.3 -f main.R -- -t TRUE`

## packagement management: renv

- module: `renv`
- init: `renv::init(repos="https://cran.rstudio.com")`
- install: `renv::install('namespace')`
- remove: `renv::remove('namespace')`

## import namepsaces: box

- module: `box`
- examples
	- `box::use()`
	- `box::use(
		dplyr[arrange, groupby, summarize],
		here[here]
	  )`

## relative path references: here

- module: `here`
- usage: `here()`
- examples
	- `here()` = project-root
	- `here("data")` = project-root/data

## importing csv: vroom

- module: `vroom`
- read: `vroom(
		file,
		delim = NULL,
		col_names = TRUE,
		col_types = NULL,
		col_select = NULL,
		id = NULL,
		skip = 0,
		n_max = Inf,
		na = c("", "NA"),
		quote = "\"",
		comment = "",
		skip_empty_rows = TRUE,
		trim_ws = TRUE,
		escape_double = TRUE,
		escape_backslash = FALSE,
		locale = default_locale(),
		guess_max = 100,
		altrep = TRUE,
		num_threads = vroom_threads(),
		progress = FALSE,
		show_col_types = NULL,
		.name_repair = "unique"
	)`
- write: `vroom_write(
			x,
			file,
			delim = "\t",
			eol = "\n",
			na = "NA",
			col_names = !append,
			append = FALSE,
			quote = c("needed", "all", "none"),
			escape = c("double", "backslash", "none"),
			bom = FALSE,
			num_threads = vroom_threads(),
			progress = FALSE
	)`

## formatting: air

- format: `air format .`

## pipe: |>

- use the native pipe: `|>`

