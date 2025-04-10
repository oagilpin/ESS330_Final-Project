---
project: 
  type: manuscript
  output-dir: docs
title: "ESS330: Final Project"
subtitle: "Intro/Methods Draft"
authors: 
  - name: Olivia Gilpin
    affiliation: Colorado State University
    roles: [writing]
    corresponding: true
  - name: Josh Ray
    affiliation: Colorado State University
    roles: [writing]
bibliography: references.bib
csl: apa-6th-edition.csl
manuscript:
  article: index.qmd
format:
  html:
    self-contained: true
execute:
  echo: true
editor: visual
---




## Final Project Proposal: Assessing the Impact of Livestock Populations on Nitrous Oxide Emissions by Livestock

Authors: Olivia Gilpin & Josh Ray

### Introduction:

The emissions of Greenhouse Gases are primarily derived from various inputs into the atmosphere, heavily including inputs from agricultural processes (Filonchyk et al., 2024). Nitrous Oxide, a main Greenhouse Gas emission, is a key contributor to climate change, driven by anthropogenic effects, which have a significant contribution to overall global emissions (Omemen & Aldbbah, 2025). Narrowing down the sources of Nitrous Oxide emissions over a temporal scale of the recent five years will show us global trends for informing stakeholders on future regulation measures. Global climate systems must be further assessed for human influence and the correlation to global Nitrous Oxide emissions on ecosystem functions (Forster et al., 2024). Understanding the environmental consequences of Nitrous Oxide emissions impacts every sector of life on earth, in addition to the environmental impacts, such as economic, social, and cultural intersections (Yılmaz & Ozaner, 2025). Through further understanding of the interconnectedness of global Nitrous Oxide emissions, stakeholders can be better informed of combating the consequences and creating interdisciplinary frameworks.

For this research, we are asking, will the livestock population of livestock have an impact on the level of Nitrous Oxide emissions? We hypothesize that as the population of livestock increases, the emissions of Nitrous Oxide will increase. The null hypothesis, if there is no significant relationship between livestock population size and Nitrous Oxide emissions, an increase in livestock population does not lead to an increase in Nitrous Oxide emissions. The objective of this study is to evaluate the relationship between livestock emissions and greenhouse gas emissions, specifically Nitrous Oxide. Through this research objective, the goal is to assess whether the population of livestock emissions will influence Nitrous Oxide emissions.

### Data Exploration:



::: {.cell}

```{.r .cell-code .hidden}
knitr::opts_chunk$set(echo = TRUE)

library(dplyr)
```

::: {.cell-output .cell-output-stderr .hidden}

```

Attaching package: 'dplyr'
```


:::

::: {.cell-output .cell-output-stderr .hidden}

```
The following objects are masked from 'package:stats':

    filter, lag
```


:::

::: {.cell-output .cell-output-stderr .hidden}

```
The following objects are masked from 'package:base':

    intersect, setdiff, setequal, union
```


:::

```{.r .cell-code .hidden}
library(janitor)
```

::: {.cell-output .cell-output-stderr .hidden}

```

Attaching package: 'janitor'
```


:::

::: {.cell-output .cell-output-stderr .hidden}

```
The following objects are masked from 'package:stats':

    chisq.test, fisher.test
```


:::

```{.r .cell-code .hidden}
library(ggplot2)
library(mediation)
```

::: {.cell-output .cell-output-stderr .hidden}

```
Loading required package: MASS
```


:::

::: {.cell-output .cell-output-stderr .hidden}

```

Attaching package: 'MASS'
```


:::

::: {.cell-output .cell-output-stderr .hidden}

```
The following object is masked from 'package:dplyr':

    select
```


:::

::: {.cell-output .cell-output-stderr .hidden}

```
Loading required package: Matrix
```


:::

::: {.cell-output .cell-output-stderr .hidden}

```
Loading required package: mvtnorm
```


:::

::: {.cell-output .cell-output-stderr .hidden}

```
Loading required package: sandwich
```


:::

::: {.cell-output .cell-output-stderr .hidden}

```
mediation: Causal Mediation Analysis
Version: 4.5.0
```


:::

```{.r .cell-code .hidden}
library(readxl)
library(tidyr)
```

::: {.cell-output .cell-output-stderr .hidden}

```

Attaching package: 'tidyr'
```


:::

::: {.cell-output .cell-output-stderr .hidden}

```
The following objects are masked from 'package:Matrix':

    expand, pack, unpack
```


:::

```{.r .cell-code .hidden}
sheet1_path <- "data/Population of Livestock.xlsx"
sheet2_path <- "data/Nitrous Oxide Emissions from Livestock Manure Management Systems.xlsx"

data1 <- read_excel(sheet1_path)
data2 <- read_excel(sheet2_path)

str(data1)
```

::: {.cell-output .cell-output-stdout}

```
tibble [42,900 × 7] (S3: tbl_df/tbl/data.frame)
 $ state_long                : chr [1:42900] "Alaska" "Alaska" "Alaska" "Alaska" ...
 $ state_short               : chr [1:42900] "AK" "AK" "AK" "AK" ...
 $ supplemental_category     : chr [1:42900] "Population of Livestock" "Population of Livestock" "Population of Livestock" "Population of Livestock" ...
 $ supplemental_subcategory_1: chr [1:42900] "Beef NOF Bull" "Beef NOF Bull" "Beef NOF Bull" "Beef NOF Bull" ...
 $ unit                      : chr [1:42900] "1,000 head" "1,000 head" "1,000 head" "1,000 head" ...
 $ value                     : num [1:42900] 1.6 1.7 1.5 1.3 1.6 1.9 2.4 2.9 2 1.9 ...
 $ year                      : chr [1:42900] "2000" "2001" "2002" "2003" ...
```


:::

```{.r .cell-code .hidden}
str(data2)
```

::: {.cell-output .cell-output-stdout}

```
tibble [117,150 × 9] (S3: tbl_df/tbl/data.frame)
 $ ghg                       : chr [1:117150] "N₂O" "N₂O" "N₂O" "N₂O" ...
 $ state_long                : chr [1:117150] "Alaska" "Alaska" "Alaska" "Alaska" ...
 $ state_short               : chr [1:117150] "AK" "AK" "AK" "AK" ...
 $ supplemental_category     : chr [1:117150] "Nitrous Oxide Emissions from Livestock Manure Management Systems" "Nitrous Oxide Emissions from Livestock Manure Management Systems" "Nitrous Oxide Emissions from Livestock Manure Management Systems" "Nitrous Oxide Emissions from Livestock Manure Management Systems" ...
 $ supplemental_subcategory_1: chr [1:117150] "Beef OF Heifers" "Beef OF Heifers" "Beef OF Heifers" "Beef OF Heifers" ...
 $ supplemental_subcategory_2: chr [1:117150] "Cattle Deep Litter (>1 month)" "Cattle Deep Litter (>1 month)" "Cattle Deep Litter (>1 month)" "Cattle Deep Litter (>1 month)" ...
 $ unit                      : chr [1:117150] "MMT CO₂e" "MMT CO₂e" "MMT CO₂e" "MMT CO₂e" ...
 $ value                     : num [1:117150] 0 0 0 0 0 0 0 0 0 0 ...
 $ year                      : chr [1:117150] "2000" "2001" "2002" "2003" ...
```


:::

```{.r .cell-code .hidden}
data1 <- data1 %>%
  clean_names() %>%
  drop_na() %>%
  distinct()

data2 <- data2 %>%
  clean_names() %>%
  drop_na() %>%
  distinct()

data_livestock <- data1 %>% 
  filter(supplemental_category == "Population of Livestock")

data_emissions <- data2 %>% 
  filter(supplemental_category == "Nitrous Oxide Emissions from Livestock Manure Management Systems")

data_livestock_aggregated <- data_livestock %>%
  group_by(year) %>%
  summarise(value_livestock = sum(value, na.rm = TRUE))

data_emissions_aggregated <- data_emissions %>%
  group_by(year) %>%
  summarise(value_emissions = sum(value, na.rm = TRUE))

data_combined <- left_join(data_livestock_aggregated, data_emissions_aggregated, by = "year")

head(data_combined)
```

::: {.cell-output .cell-output-stdout}

```
# A tibble: 6 × 3
  year  value_livestock value_emissions
  <chr>           <dbl>           <dbl>
1 1990         1708300.            13.4
2 1991         1768507.            13.5
3 1992         1827374.            13.5
4 1993         1885013.            13.4
5 1994         1950493.            14.0
6 1995         2009101.            14.3
```


:::

```{.r .cell-code .hidden}
correlation_result <- cor.test(data_combined$value_livestock, data_combined$value_emissions)
print("Correlation Analysis:")
```

::: {.cell-output .cell-output-stdout}

```
[1] "Correlation Analysis:"
```


:::

```{.r .cell-code .hidden}
print(correlation_result)
```

::: {.cell-output .cell-output-stdout}

```

	Pearson's product-moment correlation

data:  data_combined$value_livestock and data_combined$value_emissions
t = 16.043, df = 31, p-value < 2.2e-16
alternative hypothesis: true correlation is not equal to 0
95 percent confidence interval:
 0.8901091 0.9725917
sample estimates:
      cor 
0.9447263 
```


:::

```{.r .cell-code .hidden}
regression_model <- lm(value_emissions ~ value_livestock, data = data_combined)
print("Linear Regression Model Summary:")
```

::: {.cell-output .cell-output-stdout}

```
[1] "Linear Regression Model Summary:"
```


:::

```{.r .cell-code .hidden}
print(summary(regression_model))
```

::: {.cell-output .cell-output-stdout}

```

Call:
lm(formula = value_emissions ~ value_livestock, data = data_combined)

Residuals:
     Min       1Q   Median       3Q      Max 
-0.64892 -0.25862 -0.03008  0.21603  0.77258 

Coefficients:
                 Estimate Std. Error t value Pr(>|t|)    
(Intercept)     4.617e+00  6.691e-01    6.90 9.75e-08 ***
value_livestock 4.813e-06  3.000e-07   16.04  < 2e-16 ***
---
Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1

Residual standard error: 0.3438 on 31 degrees of freedom
Multiple R-squared:  0.8925,	Adjusted R-squared:  0.889 
F-statistic: 257.4 on 1 and 31 DF,  p-value: < 2.2e-16
```


:::

```{.r .cell-code .hidden}
p1 <- ggplot(data_combined, aes(x = value_livestock, y = value_emissions)) +
  geom_point(color = "blue", size = 2) +
  geom_smooth(method = "lm", se = TRUE, color = "red") +
  theme_minimal() +
  labs(title = "Livestock Population vs. Nitrous Oxide Emissions",
       x = "Livestock Population",
       y = "Nitrous Oxide Emissions")
print(p1)
```

::: {.cell-output .cell-output-stderr .hidden}

```
`geom_smooth()` using formula = 'y ~ x'
```


:::

::: {.cell-output-display}
![](index_files/figure-html/unnamed-chunk-1-1.png){width=672}
:::

```{.r .cell-code .hidden}
ggsave("scatter_regression_plot.png", plot = p1, width = 8, height = 6)
```

::: {.cell-output .cell-output-stderr .hidden}

```
`geom_smooth()` using formula = 'y ~ x'
```


:::

```{.r .cell-code .hidden}
data_combined <- data_combined %>%
  mutate(year = as.numeric(year)) %>%  # Convert year to numeric
  drop_na(year, value_emissions) %>%   # Drop rows with missing values
  arrange(year)                        # Sort by year

p2 <- ggplot(data_combined, aes(x = year, y = value_emissions)) +
  geom_line(color = "purple", linewidth = 1) +  # Use linewidth instead of size for lines
  geom_point(color = "darkgreen", size = 2) +
  theme_minimal() +
  labs(title = "Yearly Trend of Nitrous Oxide Emissions",
       x = "Year",
       y = "Nitrous Oxide Emissions")
print(p2)
```

::: {.cell-output-display}
![](index_files/figure-html/unnamed-chunk-1-2.png){width=672}
:::

```{.r .cell-code .hidden}
ggsave("trend_emissions_plot.png", plot = p2, width = 8, height = 6)
```
:::



### Proposed Methods:

Through examining the relationship between Nitrous Oxide livestock emissions and livestock populations, we would take a comparative and statistical analysis approach. This approach would include an initial establishment of emission baselines, utilizing national livestock populations and the total Nitrous Oxide emissions (Marshall & Hanson, 2024). We would then utilize the data on Nitrous Oxide livestock emissions from livestock and analyze that in comparison to the research on climate variability effects from agricultural practices (Bhatti et al., 2024; Filonchyk et al., 2024; Gołasa et al., 2021; Marshall & Hanson, 2024). A cross-comparison of Nitrous Oxide livestock emissions and livestock populations, in order to correlate livestock populations with Nitrous Oxide livestock emissions, and determine if reductions in livestock populations have an influence on the total outputs of Nitrous Oxide from livestock. For the statistical analysis, we would run a correlation analysis, a regression model, and a mediating analysis to evaluate the empirical significance of relationships.

The expected challenges of this research that we anticipate will be the lack of inclusion of the spatial dispersion of livestock populations factor, and whether that could factor into Nitrous Oxide emission rates based on livestock in a given area and their dispersal. This is an important challenge to be aware of in order to identify if higher concentrations of livestock in a smaller area of land could impact Nitrous Oxide emission rates versus a larger area of land for the same amount of livestock, but they are more dispersed versus concentrated. This limitation could be addressed by including geographic data to analyze how livestock density in different regions might affect nitrous oxide emissions. By using mapping tools and spatial regression models, we could better understand if concentrated livestock populations lead to higher emissions compared to more spread-out populations.

### References:

[@6e981dac0c1c4278bd4199223da966b8]

[@climatetrace_nodate]

[@filonchyk2024greenhouse]

[@forster2024indicators]

[@golasa2021sources]

[@nagelkerken2015global]

[@omemen2025climate]

[@ourworldindata_nodate]

[@siddik2021current]

[@yilmaz2025causality]

[@worldbank_nodate]
