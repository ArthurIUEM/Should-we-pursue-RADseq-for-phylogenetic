#### change the working directory to a folder containing the Excel files
```
setwd(‘C:/Users/arthu/OneDrive/Documents/Article/Do we need to continue with RAD-seq for phylogenetics/Data analysis/Table to analyse/useful table’)
```
#### install the ‘readxl’ package to read Excel files (necessary if not yet installed)
```
install.packages(‘readxl’)
```
#### Loads the ‘readxl’ library to use its functions
```
library(readxl)
```
#### import Excel data from a specific sheet and save in CSV format
```
data <- read_excel(‘Divergence times by method family and taxonomic rank.xlsx’, sheet = ‘Sheet1’)
write.csv(data, ‘Divergence time by method family and taxonomic rank.csv’, row.names = FALSE)
```
#### Repeats import and save for another sheet (or modified data)
```
data <- read_excel(‘Divergence times by method family and taxonomic rank.xlsx’, sheet = ‘Sheet1’)
write.csv(data, ‘Divergence time by method family and taxonomic rank V2.csv’, row.names = FALSE)
```
#### Change the working directory to access the CSV files
```
setwd(‘C:/Users/arthu/OneDrive/Documents/Article/Do we need to continue with RAD-seq for phylogenetics/Data analysis/Table to analyse/Csv table’)
```
#### Reads the CSV data, specifying the columns with missing values (‘NA’)
```
data <- read.csv(‘Divergence time by method family and taxonomic rank V2.csv’, header = TRUE, sep = ‘,’, na.strings = c(‘NA’, ‘’))
```
#### Load the ‘tidyr’ library to transform the data
```
library(tidyr)
```
#### Converts a column into a numeric type for correct statistical analysis
```
data$Deepest.Divergence.Time..My. <- as.numeric(data$Deepest.Divergence.Time..My.)
```
#### Restructure the data to a long format 
```
data_long <- pivot_long(
  data, 
  cols = -c(Deepest.Taxonomic.Rank.comparison, Method.Family), # Excludes these columns
  names_to = ‘Observation’, 
  values_to = ‘Deepest.Divergence.Time..My.’
)
```
#### Defines ordered levels for the taxonomic rank column
```
data_long$Deepest.Taxonomic.Rank.comparison <- factor(
  donnees_long$Deepest.Taxonomic.Rank.comparison, 
  levels = unique(data_long$Deepest.Taxonomic.Rank.comparison)
)
```
#### List of taxonomic rank names for customising X axis labels
```
rank_names <- c(‘Kingdom’, ‘Phylum’, ‘Clade’, ‘Class’, ‘Sub-Class’, ‘Super-Order’, ‘Order’, 
                ‘Sub-Order’, “Super-Family”, “Family”, “Sub-Family”, “Tribe”, “Sub-Tribe”, 
                ‘Genus’, “Sub-Genus”, “Specie”, “Sub-Specie”)
```
#### Create a boxplot with ggplot2 to visualise divergence times by taxonomic rank
```
ggplot(data_long, aes(x = Deepest.Taxonomic.Rank.comparison, 
                         y = Deepest.Divergence.Time..My., 
                         fill = Method.Family) +
  geom_boxplot() + # Generates a moustache box graph
  labs(
    x = ‘Taxonomic rank’, # Title of X axis
    y = ‘Divergence time (in My)’, # Y axis title
    fill = ‘Method family’ # Rename the colour legend
  ) +
  theme(
    axis.text.x = element_text(angle = 45, hjust = 1, size = 10), # Size and inclination of X labels
    axis.text.y = element_text(size = 10), # Size of Y labels
    axis.title = element_text(size = 12), # Size of axis titles
    plot.title = element_text(size = 15, face = ‘bold’), # Size and style of the title
    legend.text = element_text(size = 10), # Legend text size
    legend.title = element_text(size = 12), # Size of the legend title
    panel.background = element_rect(fill = ‘white’), # White background of the graphic
    axis.line = element_line(color = ‘black’, size = 0.5), # Thin black axis line
    panel.grid.major = element_line(color = ‘grey’, linetype = ‘dotted’), # Main grid in dotted grey
    panel.grid.minor = element_line(color = ‘grey’, linetype = ‘dashed’) # Secondary grid in discontinuous grey
  ) +
  scale_x_discrete(labels = rank_names) # Uses the names defined for the taxonomic ranks
```
