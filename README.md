# r-cheatsheet
Cheatsheet for R &amp; Rstudio

```
?<anything-here> # Help/manual for that thing
```

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

# Column Structure & Data Type
str(sample-data)

# Investigate the structure of specific columns
sample-data %>% 
  select(column-name, column-b-name) %>% 
  str()

# View a column
sample-data$column-name

# Take a Sample (dplyr)
sample_n(sample-data)
```

### Summarising Data
View as a table:
```
table(object/list)
```

Sample Size:
* Sample Size - `n()`

Measures of Centre:

_(Often has option to exclude null values by setting argument "na.rm = TRUE")_
* Mean - `mean()`
* Median - `median()`
* Mode:
```
getmode <- function(v) {
   uniqv <- unique(v)
   uniqv[which.max(tabulate(match(v, uniqv)))]
}

result <- getmode(vector)
print(result)
```

Measures of Spread:
* Range - `range()`
* Minimum - `min()`
* Maximum - `max()`
* Standard Deviation - `sd()`
* Variance - `var()`
* IQR - `IQR()`
  

Create a list of summary metrics
```
# Dplyr function
sample-data %>%
summarise(mean_dd = mean(column-name), sd_dd = sd(column-name), n = n())
```

Tabulate Categorical Data:
```
sample-data %>% 
  group_by(column-name) %>% 
  summarise(count = n())
```

### Selecting & Grouping Data
Select Top n
```
head(n)
```

Filter
```
# Dplyr function 
# AND
new-data-frame <- sample-data %>%
  filter(column-name-a == "text", column-name-b == 999)

# OR  
new-data-frame <- sample-data %>%
  filter(column-name-a == "text" | column-name-b == 999)

# Null values
new-data-frame <- sample-data %>%
  filter(!is.na(column-name))
```
Select
```
new-data-frame <- sample-data %>%
  select(column-name-a, column-name-b)
```
Group
```
sample-data %>%
  group_by(column-name-a) %>%
  summarise(mean = mean(column-name-b), sd = sd(column-name-b), n = n())
  ```

### Visualising Data
We use the `ggplot2` package for visualisation

Example Scatter Plot
```
ggplot(data = sample-data, aes(x = column-name-a, y = column-name-b)) +
  geom_point() 
```

* Scatter Plot - `geom_point()`
* Line Plot - `geom_line()`
* Histogram -`geom_histogram()` or `stat_bin()`
    * Args: # bins: `bins`, bin width: `binwidth`
* Box Plot - `geom_boxplot()`
    * Requires categorical variable on the x axis, so `factor()` if variable is not already factor type.
* Bar Chart - `geom_bar()`
    * Stacked Bar Chart: `aes(fill = column-name-a)`

## Manipulating Data
Standard Operators:
* Assignment operator `<-`
* Piping operator `%>%`

Many of the data manipulation verbs in this section come from the `dplyr` package.
* arrange()
* distinct()
* mutate()

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

### Change data types
Change a factor type column into numeric data type (via character data type).
_(Often useful for ordinal categorical variables)_
```
sample-data <- sample-data %>%
  mutate(output-column-name = as.numeric(as.character(column-name)))
```

### Feature Engineering 
* Ternary If/Else - `ifelse(column-name < 1, "FOO", "BAR"))`

## Simulations
Sampling 
```
coin_outcomes <- c("heads", "tails")  #c() - combine values into vector/list
sim_unfair_coin <- sample(coin_outcomes, size = 1000, replace = TRUE, prob = c(0.2, 0.8))
```