# Laptop Battery Life Analysis

## Project Overview

This project aims to analyze various factors affecting the battery life of a laptop. The factors under investigation include refresh rate, brightness level, resolution, and speaker volume. The analysis leverages statistical methods to determine the impact of these factors on battery life.

## Objectives

- **Objective:** To evaluate how different combinations of refresh rate, brightness, resolution, and speaker volume influence the battery life of a laptop.
- **Factors Analyzed:**
  - **Refresh Rate (A):** 60Hz (Low), 144Hz (High)
  - **Brightness (B):** Low (L1), Medium (L2), High (L3)
  - **Resolution (C):** 900x600p (Low), 1920x1080p (High)
  - **Speaker Volume (D):** Mute (Low), 75% Volume (High)

## Tools Used

- **RStudio:** For data analysis and visualization.
- **Battery Analytics Application:** For collecting battery performance data.
- **Stopwatch:** For measuring battery discharge time.
- **HP Laptop:** The hardware used for testing.

## Data Description

The dataset consists of observations of battery discharge rates under different combinations of factor levels. The data is organized into the following format:

- **Factors:** A matrix containing the observed discharge rates for various factor combinations.
- **Design Matrix:** A matrix representing the experimental design with factors and their interactions.
- **Data Frames:** For additional analyses and transformations.

## Analysis

### 1. **Design Matrix**

The design matrix includes various factors and their interactions, which are used to model the battery discharge rates. 

```r
Design.matrix <- cbind(I, A, B, AB, C, AC, BC, ABC, D, AD, BD, ABD, CD, ACD, BCD, ABCD, Factors)
```

### 2. **Main Effects Estimation**

Estimation of the main effects using the design matrix and factor levels.

```r
Feff <- t(Factors) %*% cbind(A, B, AB, C, AC, BC, ABC, D, AD, BD, ABD, CD, ACD, BCD, ABCD) / (8 * n)
```

### 3. **Half-Normal Plot**

To assess the significance of effects and interactions.

```r
library(unrepx)
G <- Design.matrix[,17]
pilotEff <- yates(G, labels = c("A", "B", "C", "D"))
hnplot(pilotEff, ID = 0)
```

### 4. **ANOVA Analysis**

Analysis of variance to determine the significance of factors and their interactions.

```r
rate.aov <- aov(rate ~ refresh_rate + brightness_level, data = data.rate)
summary(rate.aov)
```

### 5. **Post-Hoc Tests**

Performing LSD and Tukey's HSD tests to compare means and identify significant differences.

```r
library(agricolae)
lsd.test <- LSD.test(rate.aov, "rate", alpha = 0.2, group = TRUE)
tukey <- TukeyHSD(rate.aov, ordered = TRUE, conf.level = 0.9945)
```

### 6. **Model Adequacy Checking**

Evaluating model fit and residuals for adequacy.

```r
mod <- lm(Factors ~ A + B + A:B, data = data.rate)
summary(mod)
```

## Results

The analysis reveals that:
- The battery life is significantly affected by the refresh rate, brightness, and resolution.
- The interaction between these factors also plays a crucial role in determining battery discharge rates.
- The highest battery drainage is observed when all factors are set to their high levels, and the lowest when all are set to their low levels.

## Conclusion

This project provides insights into how different laptop settings impact battery life. The findings can help users optimize their laptop settings to extend battery duration.

## Repository Contents

- `project.R`: R script with the data analysis and plotting code.
- `README.md`: This file describing the project.

## Getting Started

1. Clone the repository:

   ```bash
   git clone https://github.com/karanziie1990/DAE_Course_Project.git
   ```

2. Open `project.R` in RStudio to review the analysis.

3. Install required R packages if not already installed:

   ```r
   install.packages(c("unrepx", "DoE.base", "FrF2", "agricolae", "DescTools"))
   ```

4. Run the script to reproduce the analysis and view results.
```

You can copy and paste this into a `README.md` file for your GitHub repository.
