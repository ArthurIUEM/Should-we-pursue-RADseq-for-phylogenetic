# Normality analysis and ANOVA on methods by year


#### Load data from a CSV file
```
data <- read.csv("Method type by year.csv")
```
#### Normality test (Shapiro-Wilk) for each method
```
methods <- unique(data$Method.type) # Extract the different methods
normality_results <- lapply(methods, function(method) {
# Subset the data for each method and apply the Shapiro-Wilk test
subset <- data[data$Method.type == method, "Number.of.publications"]
shapiro.test(subset) # Normality test for the subset of each method
})
```
#### Display the results of the normality test for each method
```
names(normality_results) <- methods # Associate the method name with the results
normality_results # Show results
```
#### Filter data to exclude "RR/TE" method
```
filtered_data <- data[data$MethodType != "RR/TE", ]
```
#### ANOVA to compare remaining methods (RR and TE) after excluding "RR/TE"
anova_result <- aov(Number of publications ~ MethodType, data = filtered_data)
summary(anova_result) # Summary of ANOVA to observe significant differences
