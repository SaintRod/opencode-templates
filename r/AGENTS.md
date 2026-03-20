# R

## formatting: air

- format: `air format .`

## pipe: |>

- use the native pipe: `|>`

## syntax

- prefer readable and simple over complex
- prefer base r over tidyverse unless doing so would be too verbose or complicated
- base `subset()` is fine for most use cases but `dplyr::filter()` can be easier to read if there are many filter conditions
- prefer `dplyr::groupby` and `dplyr::summarise` over base r `aggregate`
- prefer `purrr::chuck` over `obj[["name"]]` as chuck is safer
- generate empty vectors with `vector(type, length)`
