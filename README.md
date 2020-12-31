# r-cheatsheet
Cheatsheet for R &amp; Rstudio

## Packages
Install packages once from [CRAN](https://cran.r-project.org)/[Bioconductor](https://www.bioconductor.org)/[devtools](https://www.rstudio.com/products/rpackages/devtools/)
```
# CRAN
install.packages("dplyr")

# Github via devtools (Install Devtools dependecies first)
install_github()
```
Load package each time we use RStudio
```
library(dplyr)
```

## Data Exploration
### Data Loading
Consider doing data manipulation in SAS/SQL/Python/MapReduce etc., as R is not great wth manipulations of large-scale data.
#### Text File Encodings
Unless it's in ASCII, check encoding first with cmd `file` or `od`. 

**Note:** [BOMs](https://en.wikipedia.org/wiki/Byte_order_mark)
in UTF-8 files cause issues when moving between Windows vs. UNIX, so ensure you know whether it has it beforehand.

#### Loading Local Data
CSV
```
mydata <- read.csv("filename.txt")
```
More Generic
```
mydata <- read.table("filename.txt", header = TRUE, sep = "\t",
           na.strings = "NA", fileEncoding = "", encoding = "unknown")
```
Fixed-width-column formats
```
read.fwf
```
Matrices
```
A <- matrix(scan("matrix.dat", n = 200*2000), 200, 2000, byrow = TRUE)
```
### Viewing Data
We are going to use the made up `sample-data` dataset.

```
# View the Data
sample-data

# First line
first(sample-data)

# Dimensions
dim(sample-data)

# Column Names
names(sample-data)

# View a column
sample-data$column-name
```

### Summarising Data
Range
```
range(sample-data$column-name)
```

### Visualising Data
Scatter Plot
```
ggplot(data = sample-data, aes(x = column-name-a, y = column-name-b)) +
  geom_point() 
  # + geom_line() will add a line plot
```


## Manipulating Data
* Assignment operator `<-`
* Piping operator `%>%`

### Adding Variables
Adds a new column called `output-column-name` to `sample-data`
```
sample-data <- sample-data %>%
  mutate(output-column-name = column-name-a + column-name-b)
```

### Rearrange Data
Arrange a column in descending order
```
sample-data <- sample-data %>% arrange(desc(column-name))
```
