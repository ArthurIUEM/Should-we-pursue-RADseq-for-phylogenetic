#### Load the required libraries
```
library(dplyr)
library(ggplot2)
library(tidyr)
library(tidyverse)
```
#### Upload data
```
install.packages(‘readxl’)
library(readxl)
setwd(‘C:/Users/arthu/OneDrive/Documents/Article/Do we need to continue RAD-seq for phylogenetics/Data analysis/Table to analyse/useful table’)
data <- read_excel(‘Type of methods by year.xlsx’, sheet = ‘Sheet4’)
write.csv(data, ‘Type of methods by year 2.csv’, row.names = FALSE)
setwd(‘C:/Users/arthu/OneDrive/Documents/Article/Do we need to continue with RAD-seq for phylogenetics/Data analysis/Table to analyse/Table csv’)
data <- read.csv(‘Type of methods by year 2.csv’)
```
#### Check data
```
print(head(data))
```
#### Perform the chi-square test for each year
```
chi2_results <- data %>%
  rowwise() %>%
  mutate(
    chi2_result = list(chisq.test(c(RR, TE))), # Perform the test
    chi2_statistic = chi2_result$statistic, # Extract the statistic
    p_value = chi2_result$p.value # Extract the p-value
  ) %>%
  ungroup() %>%
  select(Year, chi2_statistic, p_value) # Keep only relevant columns
```
#### Display chi-squared results by year
```
print(chi2_results)
```
#### Overall test for all years combined
```
total_studies <- colSums(data[,-1]) # Exclude the ‘Year’ column
chi2_global <- chisq.test(total_studies)
```
#### Display global results
```
cat(‘Global test for all years combined:\n’)
print(chi2_global)
```
#### Viewing proportions by method
```
data_long <- data %>%
  pivot_longer(cols = c(RR, TE), names_to = ‘Method’, values_to = ‘Number’)
```
```
ggplot(data_long, aes(x = Year, y = Number, fill = Method)) +
  geom_bar(stat = ‘identity’, position = ‘dodge’) +
  labs(title = ‘Comparison of RR and TE methods over the years’,
       x = ‘Year’,
       y = ‘Number of studies’) +
  theme_minimal()
```
#### Global test after 2017
```
data_after_2017 <- data %>% filter(Year > 2017)
total_studies_after_2017 <- colSums(data_after_2017[,-1]) # Exclude ‘Year’ column
chi2_global_after_2017 <- chisq.test(total_studies_after_2017)
```
#### Show overall results after 2017
```
cat(‘\nTotal test for years after 2017:\n’)
print(chi2_global_after_2017)
```
#### Overall test for RR only
```
RR_totals <- data$RR # Select only the RR column
chi2_RR <- chisq.test(RR_totals)
```
#### Display results for RR
```
cat(‘\nTest global only for RR:\n’)
print(chi2_RR)
```
#### Global test for TE only
```
TE_totals <- data$TE # Select only the TE column
chi2_TE <- chisq.test(TE_totals)
```
#### Display results for TE
```
cat(‘\nTest global only for TE:\n’)
print(chi2_TE)
```
#### Global test for RR after 2017
```
RR_totals_after_2017 <- data_after_2017$RR
chi2_RR_after_2017 <- chisq.test(RR_totals_after_2017)
```
#### Display results for RR after 2017
```
cat(‘\nTotal test only for RR after 2017:\n’)
print(chi2_RR_after_2017)
```
#### Global test for TE after 2017
```
TE_totals_after_2017 <- data_after_2017$TE
chi2_TE_after_2017 <- chisq.test(TE_totals_after_2017)
```
#### Display results for TE after 2017
```
cat(‘\nTest global only for TE after 2017:\n’)
print(chi2_TE_after_2017)
```
