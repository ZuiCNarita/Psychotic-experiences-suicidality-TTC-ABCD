# Psychotic-experiences-suicidality-TTC-ABCD

Analysis code for the study "Longitudinal Patterns of Psychotic Experiences and Subsequent Suicidal Ideation and Self-Harm in Adolescents: A Prospective Analysis From the TTC and the ABCD Studies."

## System requirements

The analyses were conducted using R version 4.5.2.

### Software dependencies

The demonstration and analysis scripts use the following R packages:

- `missRanger`
- `geepack`
- `multcomp`
- `boot`

No non-standard hardware is required.

## Installation

Install R version 4.5.2 and the required packages:

    install.packages(c("missRanger", "geepack", "multcomp", "boot"))

Download or clone this repository before running the scripts. No additional installation or compilation is required beyond installation of R and the required packages.

## Repository structure

- `Demo Data/`: Code for generating synthetic demonstration data, introducing missing values, and performing random forest imputation.
- `IPW/`: Code for inverse probability weighting and marginal structural model analyses.
- `G-formula/`: Code for parametric g-formula analyses.

## Demo

The repository includes a synthetic data workflow to demonstrate the analytic approach without using or disclosing real participant-level data.

The demonstration workflow:

1. Generates a synthetic dataset of 1,000 observations containing variables corresponding to the longitudinal exposure, outcomes, baseline confounders, and time-varying confounders used in the analyses.
2. Introduces approximately 5% missingness into the demonstration variables.
3. Performs random forest imputation using `missRanger`.
4. Saves the completed dataset as `imputed_data.csv`.
5. Uses `imputed_data.csv` as input for the inverse probability weighting and parametric g-formula analyses.

### Running the demo

First, run the script in `Demo Data/`. This generates the synthetic dataset, introduces missing values, performs random forest imputation, and creates:

    imputed_data.csv

Next, run the corresponding scripts in `IPW/` and/or `G-formula/` using `imputed_data.csv` as the input dataset.

### Expected output

The IPW analysis estimates odds ratios and 95% confidence intervals for remitted, incident, and persistent psychotic experience patterns relative to the never pattern for subsequent suicidal ideation and self-harm.

The g-formula analysis estimates risk differences and risk ratios for remitted, incident, and persistent psychotic experience patterns relative to the never pattern for subsequent suicidal ideation and self-harm. Point estimates are calculated as the means of 1,000 bootstrap replicates, and 95% confidence intervals are calculated using the bootstrap percentile method.

## Instructions for use

### Inverse probability weighting

The IPW scripts estimate stabilized inverse probability weights for psychotic experiences at waves 1 and 2.

For wave 1, the denominator exposure model is conditional on baseline confounders, while the numerator model contains an intercept only. For wave 2, the denominator exposure model is conditional on wave 1 psychotic experiences, baseline confounders, and wave 2 time-varying confounders, while the numerator model is conditional on wave 1 psychotic experiences.

Wave-specific weights are truncated at the 99th percentile and multiplied to obtain the final weights.

Weighted marginal structural models including psychotic experiences at waves 1 and 2 and their interaction are then fitted using robust (sandwich) standard errors. Contrasts from these models are used to estimate odds ratios and 95% confidence intervals for the following longitudinal psychotic experience patterns relative to the never pattern:

- never: wave 1 = 0, wave 2 = 0 (reference)
- remitted: wave 1 = 1, wave 2 = 0
- incident: wave 1 = 0, wave 2 = 1
- persistent: wave 1 = 1, wave 2 = 1

Separate analyses are conducted for suicidal ideation and self-harm.

### Parametric g-formula

The g-formula scripts first fit models for the wave 2 time-varying confounders conditional on wave 1 psychotic experiences and baseline confounders.

Continuous time-varying confounders are simulated using fitted values and resampled model residuals. Binary time-varying confounders are simulated from Bernoulli distributions using probabilities estimated from the fitted models.

Outcome models are then fitted conditional on psychotic experiences at waves 1 and 2, their interaction, baseline confounders, and wave 2 time-varying confounders.

Counterfactual values of the time-varying confounders and outcomes are simulated under four psychotic experience patterns:

- never: wave 1 = 0, wave 2 = 0
- remitted: wave 1 = 1, wave 2 = 0
- incident: wave 1 = 0, wave 2 = 1
- persistent: wave 1 = 1, wave 2 = 1

Counterfactual risks are estimated under each pattern. Risk differences and risk ratios are calculated for the remitted, incident, and persistent patterns relative to the never pattern.

Separate analyses are conducted for suicidal ideation and self-harm.

The analysis uses 1,000 bootstrap replicates. Point estimates are calculated as the means of the bootstrap estimates, and 95% confidence intervals are calculated using the bootstrap percentile method.

### Applying the code to other data

The synthetic dataset illustrates the variable structure required by the analysis scripts. To apply the code to other datasets, users should adapt the input file paths and variable names to correspond to their data.

## Reproduction of manuscript results

The synthetic demonstration data contain no real participant-level data and are provided solely to demonstrate the analytic workflow. They do not reproduce the numerical results reported in the manuscript.

Reproduction of the numerical results reported in the manuscript requires access to the original Tokyo Teen Cohort (TTC) and Adolescent Brain Cognitive Development (ABCD) Study data. These individual-level data cannot be redistributed by the authors because they contain sensitive participant information and are subject to privacy and ethical restrictions.

## Code availability

Analysis code and synthetic demonstration data are publicly available in this repository.

## License

This code is available under the MIT License.
