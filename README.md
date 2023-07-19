# README

Notes for video recording:

## Native pipe

- Replace %>% with |>
- Restart R
- Add chunk:

mpg |> summary()

Teaching tip: When the alternative has no drawbacks to usage in the tidyverse and extends beyond it, it's great!

## Nine core packages in tidyverse 2.0.0

- Restart R
- Remove library(lubridate)
- Add the comment before current example

# easy

- Add the following

# slightly more complex

today_t <- "7/19/2023"
class(today_t)
mdy(today_t)
class(mdy(today_t))

# even more complex
today_s <- "This video is being recorded on 19 July 2023 at 1 pm ET."
class(today_s)
dmy_h(today_s, tz = "America/New_York")
class(dmy_h(today_s))

Tip: Lubridate is awesome esp since dates and times are super annoying to work with, so teach it.

## Conflict resolution in the tidyverse

- Restart before going through to reload tidyverse again
- 

Tip: Don't hide startup messages from teaching materials

Instead, address them early on to

1.  Encourage reading and understanding messages, warnings, and errors
2.  Help during hard-to-debug situations resulting from base R's silent conflict resolution

# Improved and expended join functionality

- Updates dplyr 1.1.0 onwards, mainly to better align with tools like SQL and data.table
- Change

by = c("island" = "name")

to 

by = join_by(island == name)

Tip: Prefer `join_by()` over `by` so that

1.  You can read it out loud as "where x is equal to y", just like in other logical statements where `==` is pronounced as "is equal to"
2.  You don't have to worry about `by = c(x = y)` (which is invalid) vs. `by = c(x = "y")` (which is valid) vs. `by = c("x" = "y")` (which is also valid)

## Many to many relationships

- Add

relationship = "many-to-many"

- Replace

join_by(samp_id, meas_id)

- Unmatched rows

- Say "poof" after running unmatched rows
- Add

unmatched = "error"

- Replace with 

weight_measurements |>
  inner_join(four_penguins, join_by(samp_id))

- Run through options

Tip: Exploding joins can be hard to debug for students!

Teach students how to diagnose whether the join they performed, and that may not have given an error, is indeed the one they wanted to perform. Did they lose any cases? Did they gain an unexpected amount of cases? Did they perform a join without thinking and take down the entire teaching server? These things happen, particularly if students are working with their own data for an open-ended project!

## Per operation grouping

- Typical tidyverse pipeline

penguins |>
  drop_na(sex, body_mass_g) |>
  group_by(species, sex) |>
  summarize(mean_bw = mean(body_mass_g)) |>
  ggplot(aes(x = species, y = mean_bw, fill = sex)) +
  geom_col(position = "dodge")

- Add .groups = "drop" to summarize()

- In option 2, show you can do multiple variables

.by =  c(species, sex)

- Mention group_by() is not going away, .by is an alternative. 

Tip: Worth considering changing to for teaching since downstream effects of groups can be so hard to debug.

But importantly, For new learners, pick one and stick to it.
-   For more experienced learners, particularly those learning to design their own functions and packages, it can be interesting to go through the differences and evolution.

## Quality of life improvements to case_when() and if_else()

- Write the code

penguins |>
  mutate(
    bm_cat = case_when(
      is.na(body_mass_g) ~ NA,
      body_mass_g < 3550 ~ "Small",
      between(body_mass_g, 3550, 4750) ~ "Medium",
      TRUE ~ "Large"
    )
  ) |>
  relocate(body_mass_g, bm_cat)

  - Replace

  TRUE ~ "Large"

  with

  .default = "Large"

  - Replace NA_character_ with NA

  Tip: It's a blessing to not have to introduce `NA_character_` and friends

Especially not having to introduce it as early as `if_else()` and `case_when()`. Cherish it!

Different types of `NA`s are a good topic for a course on R as a programming language, statistical computing, etc. but not necessary for an intro course.

## New syntax for separating columns

- Run the code
- Then open help for

?separate_wider_delim

And show enhanced reporting for when things fail

Tip: Excel text-to-column users are used to different approaches to "separate" if teaching folks coming from doing data manipulation in spreadsheets, leverage that to motivate different types of `separate_*()` functions, and show the benefits of programming over point-and-click software for more advanced operations like separating longer and separating with regular expressions.

## New argument for line geoms: linewidth

"Let's end on a light note!"

- Replace size with linewidth

Tip: Check the output of your old teaching materials thoroughly to not make a fool of yourself when teaching 🤣

## A bunch of other improvements over the year

Read the teaching the tidyverse in 2023 blog post for more details on all of these and other teaching related updates.

And remember that the tidyverse blog is a great place to catch up with all tidyverse (and tidymodels) progress, many of which are updates based on feature requests from users like you. For example:

-   Better **stringr** and **tidyr** alignment
-   Ability to distinguish between `NA` in levels vs. `NA` in values in **forcats**
-   New (more straightforward to learn/teach!) API for **rvest**
-   Shorter, more readable, and (in some cases) faster SQL queries in **dbplyr**
