# **Visualizing methods by taxonomy rank**

#### Load necessary libraries
```
library(ggplot2) # For graphical visualizations
```
#### Read data from CSV file
```
data <- read.csv("Method types by taxonomy rank V2.csv", header = TRUE, sep = ",")
```
#### Remove rows with missing values
```
data <- na.omit(data)
```
#### Convert data to DataFrame
```
df <- as.data.frame(data)
```
#### Display first rows of data
```
head(data)
```
#### Summarize data
```
summary(data)
```
#### Set taxonomy rank order
```
taxo_order <- c("Domain", "Kingdom", "Super-division", "Phylum", "Sub-phylum",
"Clade", "Class", "Sub-Class", "Super-Order", "Order", "Sub-Order",
"Super-Family", "Family", "Sub-Family", "Tribe", "Sub-Tribe",
"Genus", "Sub-Genus", "Species", "Sub-Specie", "Cohort")
```
#### Create a color palette with a gradient from red to blue
```
gradient_colors <- colorRampPalette(c("red","salmon" ,"blue", "steelblue"))(length(taxo_order))
```
#### Convert the column 'Taxonomic.rank' to a factor with a defined order
```
df$Taxonomic.rank <- factor(data$Taxonomic.rank, levels = taxo_order)
```
#### Create the stacked bar chart
```
ggplot(df, aes(fill=Taxonomic.rank, y=...4, x=Method.type)) +
geom_bar(position="stack", stat="identity") + # Create a stacked histogram
labs(title="Stacked histogram by method family and taxonomic rank",
x="Method family", y="Percentage of number of publications") + # Add titles
scale_fill_manual(values ​​= couleurs_gradient) + # Apply defined colors
theme(axis.text.x = element_text(angle = 45, hjust = 1)) + # Skew X-axis labels
theme(plot.title = element_text(size=20), # Title size
axis.title.x = element_text(size=15), # X-axis title size
axis.title.y = element_text(size=15), # Y-axis title size
axis.text.x = element_text(size=12), # X-axis label size
axis.text.y = element_text(size=12), # Y-axis label size
legend.title = element_text(size=15), # Legend size legend
legend.text = element_text(size=12)) # Size of legend elements
```
