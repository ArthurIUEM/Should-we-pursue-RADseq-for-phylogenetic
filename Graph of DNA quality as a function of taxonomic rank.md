#### Change the working directory to a specific folder
```
setwd(‘C:/Users/arthu/OneDrive/Documents/Article/Do we need to continue with RAD-seq for phylogenetics/Data analysis/Table to analyse/useful table’)
```
#### Install the ‘readxl’ package to read Excel files if you have not already done so
```
install.packages(‘readxl’)
```
#### Load the ‘readxl’ library to use it
```
library(readxl)
```
#### Reads a specific sheet (‘Sheet1’) from the Excel file and assigns it to the `data` object
```
data <- read_excel(‘Taxonomic rank by degraded DNA.xlsx’, sheet = ‘Sheet1’)
```
#### Saves the data in CSV format in the current directory
```
write.csv(data, ‘Taxonomic rank by degraded DNA.csv’, row.names = FALSE)
```
#### Change the working directory to another folder to manage CSV files
```
setwd(‘C:/Users/arthu/OneDrive/Documents/Article/Do we need to continue RAD-seq for phylogenetics/Data analysis/Table to analyse/Table csv’)
```
#### Reads the CSV file and assigns it to the `data` object
```
data <- read.csv(‘Taxonomic rank by degraded DNA.csv’, header = TRUE, sep = ‘,’)
```
#### Converts the data into a DataFrame object (even if it already is)
```
df <- as.data.frame(data)
```
#### Displays the first few lines of the DataFrame to check the data structure
```
head(data)
```
#### Displays a statistical summary of the numeric columns in the DataFrame
```
summary(data)
```
#### Loads the ‘ggplot2’ library to create graphical visualisations
```
library(ggplot2)
```
#### Redefine the levels of a column (DNA quality) with new labels
```
new_names_fill <- c(‘Degraded’, ‘Not degraded’)
df$DNA.quality <- factor(df$DNA.quality, levels = unique(df$DNA.quality), labels = new_names_fill)
```
#### Defines a custom colour palette
```
custom_colours <- c(‘coral2’, ‘steelblue’)
```
#### Creates a stacked bar chart using ggplot2
```
ggplot(df, aes(fill = DNA.quality, y = Number.of.publications, x = Taxonomic.rank)) + 
  geom_bar(position = ‘stack’, stat = ‘identity’) + # Stacks the bars according to DNA quality and displays the total for each taxonomic rank
  theme_minimal() + # Applies a minimalist graphical theme
  labs(
    x = ‘Taxonomic rank’, # X axis title
    y = ‘Number of publications’, # Y axis title
    title = ‘Taxonomic rank analysed depending on the state of DNA degradation’, # Graph title
    fill = ‘State of degradation’ # Colour legend
  ) +
  theme(
    plot.title = element_text(hjust = 0.5, size = 22, face = ‘bold’), # Title centring and formatting
    axis.title.x = element_text(size = 20), # Size of the X axis title
    axis.title.y = element_text(size = 20), # Size of the Y axis title
    axis.text.x = element_text(angle = 45, hjust = 1, size = 18), # Angle and size of the labels on the X axis
    axis.text.y = element_text(size = 18), # Size of labels on the Y axis
    legend.title = element_text(size = 20), # Size of the legend title
    legend.text = element_text(size = 18) # Size of the legend text
  ) +
  scale_fill_manual(‘Method family’, values = custom_colours) # Applies the colours defined in `custom_colours`
```
