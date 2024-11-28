#### change the working directory to point to the Excel files to be analysed
```
setwd(‘C:/Users/arthu/OneDrive/Documents/Article/Do we need to continue with RAD-seq for phylogenetics/Data analysis/Table to analyse/useful table’)
```
#### install the ‘readxl’ package for manipulating Excel files (necessary if not yet installed)
```
install.packages(‘readxl’)
```
#### Loads the ‘readxl’ library
```
library(readxl)
```
#### installs and loads the ‘ggplot2’ package for creating graphs
```
install.packages(‘ggplot2’)
library(ggplot2)
```
#### Reads an Excel file (first sheet) and saves the data in CSV format
```
data <- read_excel(‘Type of methods by year.xlsx’, sheet = ‘Sheet1’)
write.csv(data, ‘Type of methods by year.csv’, row.names = FALSE)
```
#### Repeats the import and save for another sheet (or modified version of the data)
```
data <- read_excel(‘Type of methods by year.xlsx’, sheet = ‘Sheet2’)
write.csv(data, ‘Type of methods by year V2.csv’, row.names = FALSE)
```
#### Change the working directory to point to the folder containing the CSV files
```
setwd(‘C:/Users/arthu/OneDrive/Documents/Article/Do we need to continue with RAD-seq for phylogenetics/Data analysis/Table to analyse/Csv table’)
```
#### Reads the CSV file with the method type data by year
```
data <- read.csv(‘Type of methods by year.csv’, header = TRUE, sep = ‘,’)
```
#### Transforms the data into a ‘data frame’ format for easy manipulation
```
df <- as.data.frame(data)
```
#### Define custom colours for each type of method
```
custom_colours <- c(‘RR’ = ‘red’, ‘RR/TE’ = ‘blue’, ‘TE’ = ‘darkgreen’)
```
#### Creation of a graph with ggplot2 to visualise method types over the years
```
ggplot(df, aes(
  x = Year, # X axis: years
  y = Number.of.publications, # Y axis: number of publications
  color = Type.of.methods, # Colour of the curves according to the type of method
  group = Type.of.methods # Group the points by type of method to link them together
)) +
  geom_line() + # Adds lines connecting the points
  geom_point() + # Adds points for data values
  labs(
    x = ‘Year’, # Name of X axis
    y = ‘Number of publications’, # Name of Y axis
    title = ‘Number of publications according to the family of methods used and the year of publication’, # Graph title
    fill = ‘Method family’ # Name of fill legend
  ) +
  theme_minimal() + # Applies a minimalist theme to the graph
  theme(
    plot.title = element_text(hjust = 0.5, size = 16.5, face = ‘bold’), # Centred and formatted title
    axis.title.x = element_text(size = 18), # X axis title size
    axis.title.y = element_text(size = 18), # Size of Y axis title
    axis.text.x = element_text(size = 16), # Size of the X axis graduations
    axis.text.y = element_text(size = 16), # Size of the Y axis graduations
    legend.title = element_text(size = 18), # Size of the legend title
    legend.text = element_text(size = 16) # Size of the legend text
  ) +
  scale_color_manual(values = custom_colours) # Applies the custom colours for each type of method
```
