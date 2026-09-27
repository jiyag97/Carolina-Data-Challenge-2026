# Can You Fix It?

**Can You Fix It?** is an interactive school stress intervention planning tool. It uses the Open Data Index for Schools (ODIS) to help people explore how community conditions differ across U.S. high schools and estimate which local interventions could improve a school’s composite score.

The project is designed to make school and community data easier to explore and to make planning assumptions visible. Its recommendations are estimates for discussion and prioritization, not guaranteed outcomes or a substitute for local expertise.

## What it does

- Explore school conditions on a national map and compare places with different composite scores.
- Select a high school to review its composite score, indicator values, and national percentile context.
- See which intervention levers meet the calculator’s eligibility rules.
- Build an estimated plan toward a target score reduction, or adjust intervention levels and see their modeled cost and score impact.

## Data and methodology

The calculator uses ODIS school-level data for 23,599 U.S. high schools. It also retrieves 2023 county population estimates from the U.S. Census American Community Survey (ACS) and applies state Regional Price Parities (RPP) to adjust estimated intervention costs.

### Score effects

ODIS combines five domains with equal weight. Within each domain, indicators are averaged. The calculator follows those published weights to estimate how changing an indicator changes the composite score, then applies ODIS’s 0–100 display scale. The modeled change to the index is therefore formula-based; it does **not** establish that an intervention will cause a particular change in student stress.

### Eligibility and improvement limits

A lever is recommended when a school’s indicator is above the model’s target. The current targets are park access 4, broadband 9, healthcare 2, violent crime 8, and SNAP 31. SNAP also requires a poverty score above 30. The calculator limits improvement to the gap between the school’s current value and the target.

### Cost estimates

Base estimates are applied per indicator point and adjusted for local population and prices:

| Intervention | Base estimated cost per point |
| --- | ---: |
| Park access | $400,000 |
| Broadband | $120,000 per year |
| Healthcare access | $320,000 |
| Violent crime reduction | $90,000 |
| SNAP-related income support | $700,000 per year |

The county population adjustment is square-root scaled relative to 85,000 residents, bounded between 0.5× and 3×, and multiplied by the state RPP factor. The planning algorithm ranks eligible levers by modeled score reduction per estimated dollar and allocates toward the requested reduction, subject to each lever’s available gap and a 40% per-lever contribution cap.

## Important limitations

- The composite is an index of community conditions, not a direct measurement of student stress.
- The score calculation follows the index formula, but the connection between spending and indicator change is uncertain. Cost estimates have different levels of evidence and should be treated as planning assumptions.
- Costs and effects are generalized across places; local program design, rural or urban context, and implementation capacity can differ.
- The population scaling and some cost assumptions are modeling choices. The calculator also compares some recurring yearly costs with largely one-time costs.
- Park-access data are missing for many schools, and violent-crime data are also incomplete; affected levers may be unavailable.
- A requested score reduction may not be fully achievable from a school’s eligible levers and available gaps. Always review the plan and assumptions before interpreting its projected result.

## Is this AI?

The current calculator is a transparent, rule-based decision-support tool. It does not use a trained machine-learning model or generate intervention recommendations with generative AI. Its social-good purpose is to make public data and planning assumptions more accessible, so communities can explore possible priorities and ask better-informed questions.

## Run locally

The calculator source included here is an R Markdown document with a Shiny runtime.

1. Install R and the required packages:

   ```r
   install.packages(c("tidyverse", "tidycensus", "shiny", "rmarkdown"))
   ```

2. Place the ODIS dataset at `data/index_scores_v3_2026.csv`.
3. Configure a Census API key for the ACS county population lookup. Keep the key private and do not commit it to GitHub:

   ```r
   tidycensus::census_api_key("YOUR_CENSUS_API_KEY", install = TRUE)
   ```

4. From the project directory, launch the app:

   ```r
   rmarkdown::run("can_you_fix_it.Rmd")
   ```

The application also needs an internet connection to retrieve ACS population estimates when it starts.

## Sources and acknowledgments

- Open Data Index for Schools (ODIS), Johns Hopkins University: school indicators and composite-score methodology.
- U.S. Census Bureau, American Community Survey: county population estimates.
- U.S. Bureau of Economic Analysis: Regional Price Parities used in the cost adjustment.
- Published program and intervention cost studies summarized in the project’s model justification.

