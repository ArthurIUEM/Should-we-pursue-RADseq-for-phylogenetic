# Manipulation and transformation of genome size data

### **Part 1: Replacing species names with genome sizes**

#### Load the necessary libraries
```
library(readxl) # To read Excel files
library(writexl) # To write Excel files
library(dplyr) # To manipulate the data
```
#### Define the file paths
```
excel_file <- "Genome size.xlsx" # Excel file with the initial data
text_file <- "species_with_genome_size.txt" # Text file containing the genome sizes
output_file <- "Genome_size_modified.xlsx" # Output Excel file
```
#### Load the "Species by article" sheet
```
espece_par_article <- read_excel(excel_file, sheet = "Species by article")
```
#### Load the text file containing the genome sizes genomes
```
genome_sizes <- read.delim(
text_file, header = FALSE, sep = "\t",
col.names = c("Species", "Size")
)
```
#### Identify columns starting with "Study group"
```
columns_group <- grep("^Study group", names(species_by_article), value = TRUE)
```
#### Replace species names with their genome sizes
```
for (col in columns_group) {
species_by_article[[col]] <- ifelse(
species_by_article[[col]] %in% genome_sizes$Species,
genome_sizes$Size[match(species_by_article[[col]], genome_sizes$Species)],
species_by_article[[col]]
)
}
```
#### Load the second "Genome size" sheet for the conserve
```
genome_size <- read_excel(excel_file, sheet = "Genome_size")
```
#### Save the data in a new Excel file
```
write_xlsx(
list(
"Species by article" = species_by_article,
"Genome_size" = genome_size
),
output_file
)
```
#### Confirmation message
```
cat("Modified file saved under the name:", output_file, "\n")
```
### Part 2: Adding a 'Method family' column

#### Load the necessary libraries
```
library(readxl)
library(writexl)
```
#### Load the data from "Meta-analysis.xlsx", sheet "Sheet1"
```
meta_analysis <- read_excel("Meta-analysis.xlsx", sheet = "Sheet1")
```
#### Load the data from "modified_genome_size.xlsx"
```
genome_size <- read_excel("modified_genome_size.xlsx")
```
#### Add a "Method Family" column using the matches
```
taille_genome$`Famille de méthode` <- meta_analyse$`Method Family`[
match(taille_genome$`First Author`, meta_analyse$`First Author`)
]
```
#### Delete rows containing only NAs
```
taille_genome <- taille_genome[rowSums(is.na(taille_genome)) < ncol(taille_genome), ]
```
#### Save the modified file
```
write_xlsx(taille_genome, "taille_genome_modifie_avec_method_family_sans_NA.xlsx")
```
#### Confirmation message
```
cat("The file 'taille_genome_modifie_avec_method_family_sans_NA.xlsx' has been successfully created.\n")
```
### Part 3: Transforming the table into format long

#### Load the necessary libraries
```
library(readxl)
library(tidyr)
library(writexl)
```
#### Load data from Excel file
```
file_path <- "taille_genome_modifie_avec_method_family_sans_NA.xlsx"
data <- read_excel(file_path)
```
#### Identify the columns containing the genome sizes (starting with "Groupe étude")
```
genome_columns <- grep("^Groupe étude", names(data), value = TRUE)
```
#### Transform the table into long format
```
long_format <- data %>%
pivot_longer(
cols = all_of(genome_columns),
names_to = "Groupe étude",
values_to = "Taille de génie"
)
```
#### Delete the rows where the genome size is NA
```
long_format <- long_format[!is.na(long_format$`Taille de génie`), ]
```
#### Save the transformed table in a new Excel file
```
output_path <- "tableau_famille_taille_genome.xlsx"
write_xlsx(long_format, output_path)
```
#### Confirmation message
```
cat("The transformed table has been saved in:", output_path, "\n")
```
