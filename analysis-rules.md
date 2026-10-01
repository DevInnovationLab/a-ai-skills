# Analysis code

## General

- Analysis scripts never create or change variables. If a variable is
  missing, add it to the construction script.
- **Check the data dictionary before using a variable.** Don't assume what a
  variable measures from its name or label. Take into account:
  - unit of measurement (litres per day, rupees, share from 0 to 1)
  - unit of observation (household, person, village) and the ID that
    identifies it
- **One source of truth.** Define samples, outcomes and control lists once,
  in one settings file that every script loads (a globals `.do` file or a
  sourced `.R` file). Only retype a control list or a sample condition in a
  second script if it's unique to that analysis.
- As a general rule, **define the estimation sample once**, at the top of
  the script, right after loading the data. Only subset the data halfway through
  a script if the sample varies for different models.
- **Categories are not numbers.** Use factor notation for categorical
  variables (Stata: `i.district`; R: `factor(district)`, including
  `haven`-labelled variables), and set the base category on purpose
  (`ib3.district`, `fct_relevel()`). Don't build dummies by hand.
- **Interactions:** Unless specified by the user, use Stata `i.treat##c.age`
  (write`c.` for continuous variables) not `#`. R: `treat * age`, not `treat:age`.

## Exploratory reports

- Exploratory results go in a literate document (Quarto, RMarkdown if R, LaTeX if Stata)
  that renders in one command.
- **Never type a result.** Every number in the text must be inline code
  (`` `r nrow(hh)` ``) or a macro written by the code (`\Nhh` in LaTeX).
  Every table and figure must be produced by the code when the document
  renders or the script runs. A number typed by hand, by you or by me, is a
  bug.
- For LaTeX/Overleaf reports: export tables as `.tex`, figures as `.png`,
  and key numbers as LaTeX macros, and include them with `\input{}` and
  `\includegraphics{}`. Never paste results into the `.tex` file.
- Write the narrative around results if asked, but mark interpretations as
  drafts for the team to review. Don't claim an effect is meaningful, or
  explain a surprising result, without flagging it as a question.

## Research decisions are not yours to make

Don't choose any of the following on your own. List them as open questions,
with the options you see and what each would change:

- definitions of indicators and outcomes
- cut-offs, trimming, winsorizing or top-coding rules, and how outliers are
  defined (the PIs make the final call; see "Imputation and outlier
  treatment")
- how to handle missing values: imputation methods, minimum number of
  components, missing-baseline indicators
- sample restrictions and which observations to drop
- weights and the level an average is taken at
- specifications, controls and fixed effects beyond what was asked
