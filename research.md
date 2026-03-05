# Purpose

The purpose of this file is for me to validate your understanding of the project, request, etc.

Read this folder in-depth, understand how it works deeply, what it does and all its specificities.
When that’s done, write a detailed report of your learnings and findings in this file under the research header.
Use step-by-step reasoning and discuss your reasoning.
Be succinct but clear and explicit - leave no ambiguity.
Leverage the project structure in your research.

##General guidance

- `src`
	- houses functions
	- functions are often grouped together, for example: `src/aws/`, `src/logistics`, etc.
- `scripts`
	- imports functions and combines the functions in a logical way
	- sometimes `scripts` is broken out by scope such as by pipeline a, pipeline b, etc.
- `main`
	- is the entry point to the codebase
	- usually `main` imports `scripts` individually or an abstraction of `scripts`
- `notebooks`
	- houses one-off research
	- usually of type quarto markdown `.qmd`
- `documentation`
	- houses API documentation
	- I often programmatically generate documentation based on doc strings
- `docs` or `pages` are GitHub and GitLab publishing conventions, ignore these directories


# Research
