#### Load the necessary libraries
```
library(ggplot2) # To create graphs
library(tidyr) # To transform and manipulate data
library(readxl) # To read Excel files
```
### Step 1: Load the data
#### Read an Excel file containing genome sizes
```
data <- read_excel("genome_size_family_table.xlsx", sheet = "Sheet1")
```
#### Save the data as CSV for easy reuse
```
write.csv(data, "genome_size_family_table.csv", row.names = FALSE)
```
#### Load the data from the CSV file
data <- read.csv("genome_size_family_table.csv", header = TRUE)

### Step 2: Restructure the data
#### The data must be transformed into a "long" format for ggplot2
```
data_long <- pivot_longer(
data,
cols = -1, # All columns except the first are grouped
names_to = "Method", # The names of the grouped columns will go into a "Method" column
values_to = "Genome_size" # The corresponding values ​​will go into a "Genome_size" column
)

#### Rename the first column to "Family" for clarity
colnames(data_long)[1] <- "Family"

### Step 3: Create a boxplot
ggplot(data_long, aes(x = Family, y = Genome_size)) + # Set the axes
geom_boxplot() + # Add a boxplot
labs(
title = "Genome Size Boxplot by Family", # Add a title
x = "Method Family", # X-axis label
y = "Genome Size (in Gb)" # Y-axis label
) +
theme_minimal() + # Apply a minimalist theme
theme(axis.text.x = element_text(angle = 45, hjust = 1)) # Rotate labels to avoid overlapping
```
