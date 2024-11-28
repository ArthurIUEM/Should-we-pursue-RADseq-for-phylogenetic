#### Load the necessary libraries
```
library(ggplot2) # To create graphics
```
#### Read the CSV file
```
data <- read_excel(‘Type of enzyme used.xlsx’, sheet = ‘Sheet1’)
write.csv(data, ‘Type of enzyme used.csv’, row.names = FALSE)
data <- read.csv(‘Type of enzyme used.csv’)
```
#### Check the first few rows of the table to make sure the data is correct
```
head(data)
```
#### Create a bar chart
```
ggplot(data, aes(x = reorder(Restriction.enzyme, -Number.of.publication), y = Number.of.publication)) +
  geom_bar(stat = ‘identity’, fill = ‘skyblue’) +
  labs(title = ‘Number of publications mentioning each restriction enzyme’,
       x = ‘Enzyme’,
       y = ‘Number of publications’) +
  theme_minimal() +
  theme(axis.text.x = element_text(angle = 45, hjust = 1))
```
