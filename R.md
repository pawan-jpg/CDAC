## Part 1: Data Types & Structures

### 1. Vectors and Basic Data Types

```R
vect = c(1L, 3L, 5L, 8L)
vect
class(vect)
as.numeric(vect)
```

- `c()` creates a vector.
- `1L` specifies integer values.
- `class()` gives the data type.
- `as.numeric()` converts data to numeric (double).

---

### 2. Character Vectors and Indexing

```R
z <- c("abc", "xyz", "mno", "pqr")
z
length(z)
names(z) <- c(1, 2, 3, 4)
z[-2]
```

- Vectors can hold strings.
- `length()` returns the number of elements.
- `names()` assigns names to each element.
- Negative indexing excludes specified positions.

---

### 3. Lists in R

```R
l <- list("a", 1L, 1.5, TRUE)
str(l)
```

- `list()` can contain elements of different types.
- `str()` gives internal structure of the list.

```R
d <- list(list("a",1L, 1.5, TRUE), list(1L, 3L, 5L, 8L))
d
```

- Nested lists (lists inside lists).

---

### 4. Named Lists

```R
city <- list("MH" = 1, "MP" = 2, "CH" = 3)
city
```

- Named lists can be accessed by keys like dictionaries.

---

### 5. Installing & Loading Packages

```R
install.packages("tidyverse")
install.packages("devtools")

library(tidyverse)
library(lubridate)
```

- `install.packages()` installs external libraries.
- `library()` loads them into the session.

---

### 6. Date & Time with lubridate

```R
today()
now()
```

- `today()` returns current date.
- `now()` returns current date-time.

```R
dd <- ymd("2023-01-20")
class(dd)
ymd(20250409)
oops <- ymd_hms(now())
as_date(oops)
```

- `ymd()`, `ymd_hms()` parse strings into dates.
- `as_date()` extracts date from datetime.

---

### 7. Matrices in R

```R
a = 1:5
b = 46:50

m1 = cbind(a, b) # column bind
m2 = rbind(a, b) # row bind
```

- `cbind()` combines by columns.
- `rbind()` combines by rows.

```R
dim(m1)
dim(m2)
```

- `dim()` shows dimensions (rows x columns).

---

### 8. Vector Recycling and Warnings

```R
a = c(3, 5, 6, 7)
f = c(2, 4, 1)

c = a + f
```

- R recycles shorter vectors.
- Warning is shown if lengths are not divisible.

---

### 9. Arrays

```R
h = array(dim = c(3))
h[1] = 3
h[2] = 55
h[3] = 90
```

- `array()` creates multi-dimensional containers.

---

### 10. Data Frames

```R
a = c("A", "B", "C")
b = c(6, 5, 4)

df = data.frame(a, b)
View(df)
```

- `data.frame()` creates tabular data.
- `View()` opens it in spreadsheet format (RStudio).

---

### 11. Column Names

```R
s = list(p = c(4,5,2), q = c(3,2), r = TRUE)
names(s)
colnames(s)
```

- `names()` works for both lists and data frames.
- `colnames()` is only for data frames.
  
## Part 2: Reading & Writing Data

---

### 2.1 Reading CSV Files

```R
c2018 = read.csv("E:/datasetscars2018.csv")
View(c2018)

boston = read.csv("E:/Pawan/CAP_May24-main_R_Data/Datasets/Datasets/boston.csv")
View(boston)

unique(c2018$Transmission)
```

- `read.csv()` reads comma-separated files.
- `View()` opens the data in tabular form.
- `unique()` shows unique values in a column.

---

### 2.2 Set Working Directory

```R
setwd("E:/datasets")
```

- Sets the default path for reading/writing files.

---

### 2.3 Reading CSV with Strings as Factors

```R
c2018 = read.csv("cars2018.csv", stringsAsFactors = TRUE)
View(c2018)
```

- Converts character columns to factor types.

---

### 2.4 Understanding Data Structure

```R
str(boston)
str(c2018)

c2018$Transmission = factor(c2018$Transmission)
```

- `str()` shows internal structure.
- Convert column to factor explicitly.

---

### 2.5 Handling Files without Headers

```R
bolyw = read.csv("Bollywood_2015_2.csv", header = FALSE)
colnames(bolyw) = c("movie_name", "BO", "Budget", "verdict")
View(bolyw)
```

- Use `header=FALSE` if the file lacks headers.
- Assign column names manually.

---

### 2.6 Reading Semicolon-Separated CSVs

```R
diamonds = read.csv2("Diamonds.csv")
ruby = read.csv2("Diamonds.csv", sep = ";", dec = ",")
```

- `read.csv2()` is for semicolon-separated files (often used in Europe).
- Specify custom separator and decimal if needed.

---

### 2.7 Reading Excel Files

```R
library(readxl)

psr = read_excel("pressure.xlsx")
qal = read_excel("quality.xlsx", range = "A1:C6", sheet = "quality")
qal2 = read_excel("quality.xlsx", range = "H1:J19", sheet = "quality")
```

- `read_excel()` loads Excel `.xlsx` files.
- Use `range` and `sheet` to specify data location.

---

### 2.8 Exporting Data to CSV and Excel

```R
data("Formaldehyde")
write.csv(Formaldehyde, "E:/datasets/psb.csv")
write.csv(Formaldehyde, "E:/datasets/pssb.csv", row.names = FALSE)
```

- `write.csv()` saves a dataframe to disk.
- `row.names = FALSE` excludes row indices.

```R
library(writexl)
write_xlsx(Formaldehyde, "E:/datasets/pb.xlsx")
```

- `write_xlsx()` writes data directly to Excel.

---

### 2.9 Subsetting Data

#### Vectors

```R
v = c(56,89,10,90,45,67,22,30,69)
v[v > 50]
```

- Filter values using logical conditions.

#### Matrices

```R
m = matrix(1:12, 3, 4)
m[, 2]  # 2nd column
m[2, ]  # 2nd row
m[2, 3] # element at (2,3)
```

---

#### Data Frames

```R
Formaldehyde[c(1, 4, 6), ]
```

- Subset specific rows using indexing.

---

### 2.10 The subset() Function

```R
ss = subset(diamonds, carat > 4)
sc = subset(diamonds, cut == 'Ideal')
sp = subset(diamonds, cut == 'Ideal' & price > 2000)
```

- `subset()` simplifies conditional filtering.
- Use `&` for multiple conditions.

## Part 3: Control Structures & Functions

---

### 3.1 if, else Conditionals

```R
s = 90

if (s > 30) {
  t = 9
  s = 20
} else {
  t = 16
  s = 30
}

print(t)
print(s)
```

- if-else blocks evaluate conditions.
- Braces `{}` are required when using multiple statements.

---

### 3.2 Taking User Input

```R
p <- as.numeric(readline("Principal: "))
n <- as.numeric(readline("No. of Years: "))
t <- as.numeric(readline("Rate of Interest: "))

si = (p * n * t) / 100

if (si > 50000) {
  si = si * 0.1
}
```

- `readline()` reads input as text.
- Wrap with `as.numeric()` to convert to number.

---

### 3.3 String Output and Formatting

```R
paste("Tax:", si)
sprintf("TAX: %d", si)
paste("PG", "DBDA", sep = "-")
paste0("PG", "DBDA")
```

- `paste()` joins elements with space or custom separator.
- `sprintf()` formats output like C-style strings.

---

### 3.4 Random Number Example

```R
x <- runif(1, 0, 10)  # random number between 0 and 10

if (x > 3) {
  y <- 10
} else {
  y <- 0
}

print(y)
print(x)
```

---

### 3.5 for Loops

```R
for (i in 1:10) {
  print(i)
}

for (i in c(3, 4, 5, 6)) {
  print(i * i)
}
```

- `for` loops iterate over a sequence or vector.

---

### 3.6 Factorial with for Loop

```R
sum <- 1
for (i in 1:10) {
  sum = sum * i
}
print(sum)

factorial(10)  # built-in factorial
```

---

### 3.7 Sequences

```R
seq(10, 1, -1)      # decreasing sequence
seq(1, 10, 1)       # increasing
```

---

### 3.8 while Loops

```R
count <- 1
while (count < 10) {
  print(count)
  count <- count + 1
}

i = 1
while (i <= 50) {
  print(i)
  i <- i + 5
}
```

---

### 3.9 break and next

```R
for (i in seq(1, 50, 5)) {
  print(i)
  if (i > 30) break
}

for (i in seq(1, 4)) {
  if (i == 3) next
  print(i)
}
```

- `break`: exits the loop early.
- `next`: skips the current iteration.

---

### 3.10 Creating Functions in R

#### Syntax

```R
function_name <- function(arg1, arg2) {
  # statements
}
```

#### Example: Add Function

```R
add <- function(a, b) {
  a + b
}
add(3, 4)
```

#### Example: Fahrenheit to Celsius

```R
fah_to_cel <- function(F) {
  (F - 32) * (5 / 9)
}
fah_to_cel(100)
```

#### Simple Interest Calculator

```R
SI <- function(p, n, r) {
  (p * n * r) / 100
}
SI(10000, 10, 10)
```

#### Vectorized SI Function

```R
p = c(100000, 500000, 430000, 5600000)
n = c(3, 5, 6, 9)
r = c(8, 8, 8.5, 6)

SI(p, n, r)  # returns a vector of calculated interest
```
## Part 4: Data Manipulation with tidyverse

---

### 4.1 Loading Data and Creating Tibbles

```R
library(dplyr)

data("mtcars")
class(mtcars)

tbl_cars = as_tibble(mtcars)
View(tbl_cars)
```

- `as_tibble()` gives a cleaner table format.
- Tibble is a modern version of data frame.

---

### 4.2 Sorting Data with arrange()

```R
arrange(mtcars, mpg)               # ascending
arrange(mtcars, desc(mpg))         # descending
arrange(mtcars, cyl, desc(mpg))    # multi-level sort
```

- `arrange()` sorts rows by one or more columns.

---

### 4.3 Selecting Columns with select()

```R
select(mtcars, mpg, wt, am)         # specific columns
select(mtcars, mpg:wt)              # range of columns
select(mtcars, contains("t"))       # column name contains
select(mtcars, starts_with("d"))    # starts with
select(mtcars, ends_with("s"))      # ends with
```

- Dynamically pick columns using conditions.

---

### 4.4 Filtering Rows with filter()

```R
filter(mtcars, mpg > 30)

select(mtcars, mpg, wt, hp, am) %>%
  filter(mpg > 30)
```

- Filters rows based on conditions.

---

### 4.5 Renaming Columns

```R
rename(mtcars, mileage = mpg, weight = wt)
```

- `rename(new_name = old_name)`.

---

### 4.6 Creating New Columns with mutate()

```R
mutate(mtcars, ratio = mpg / hp)
mutate(mtcars, ratio = round(mpg / hp, 2))

select(mutate(mtcars, ratio = mpg / hp), mpg, wt, ratio, hp)
```

- Adds new columns, can modify existing ones.

---

### 4.7 Summarising Data with summarise()

```R
summarise(mtcars, avg_disp = mean(disp))
summarise(mtcars, avg_disp = mean(disp), sd_disp = sd(disp))
```

- Use to compute summary statistics.

---

### 4.8 Grouping with group_by()

```R
group_by(mtcars, cyl) %>%
  summarise(avg_mpg = mean(mpg))
```

- Group by one or more columns before summarizing.

---

### 4.9 Chaining with %>% and |>

```R
mtcars %>%
  select(mpg, wt, hp, am) %>%
  filter(mpg > 30)

mtcars |> 
  select(mpg, wt, hp, am) |>
  filter(mpg > 30)
```

- `%>%` and `|>` (base pipe) allow readable data workflows.

---

### 4.10 Cross-tabulation with table()

```R
table(survey$Sex, survey$Smoke, useNA = "ifany")
```

- Count of combinations.
- `useNA = "ifany"` includes NA rows if any.

---

### 4.11 Add Totals with addmargins()

```R
addmargins(table(survey$Exer, survey$Sex))
```

- Adds row and column totals.

---

### 4.12 Logical and Math Helpers

```R
log(2)       # natural log
log1p(2)     # log(1+x)
exp(1)       # e^1
expm1(1)     # e^x - 1
```

---

### 4.13 ifelse() - Vectorized Conditional

```R
mtcars$milage = ifelse(mtcars$mpg > 25, "Good Mileage", "OK Mileage")
```

- Works like a ternary operator.
- Applies condition to each element in a vector.

---
## Part 5: Data Cleaning & Transformation

---

### 5.1 Survey Data: Exploring Categorical Columns

```R
survey <- read.csv("survey.csv", stringsAsFactors = TRUE)

table(survey$Sex)
table(survey$Smoke)
table(survey$Exer)
```

- `table()` gives frequency counts.
- Good for quick insights into factor or character columns.

---

#### Handling Missing Values in Tables

```R
table(survey$Smoke, useNA = "ifany")
table(survey$Sex, useNA = "always")
```

- `useNA = "ifany"` includes NA if present.
- `useNA = "always"` includes NA column even if none exist.

---

#### Cross-tabulation

```R
table(survey$Smoke, survey$Exer)
table(survey$Exer, survey$Sex, useNA = "ifany")
```

- 2D summaries (similar to pivot tables).

---

#### Add Totals (Margins)

```R
addmargins(table(survey$Exer, survey$Sex))
```

---

### 5.2 Penguin Dataset: Overview

```R
library(palmerpenguins)
View(penguins)
```

- Built-in dataset with body measurements of penguin species.

---

#### Sorting Data

```R
penguins %>% arrange(-bill_length_mm)
```

- Use `-` to sort in descending order.

---

#### Group-wise Summary

```R
penguins %>%
  group_by(island) %>%
  drop_na() %>%
  summarize(mean_bill = mean(bill_length_mm),
            max_bill = max(bill_length_mm),
            min_bill = min(bill_length_mm),
            sd_bill = sd(bill_length_mm),
            var_bill = var(bill_length_mm))
```

- `drop_na()` removes missing values.
- `group_by()` + `summarize()` = grouped stats.

---

#### Multiple Grouping Levels

```R
penguins %>%
  group_by(species, island) %>%
  drop_na() %>%
  summarize(max_bill = max(bill_length_mm),
            min_bill = min(bill_length_mm))
```

---

#### Filtered Summaries

```R
penguins %>% filter(species == "Gentoo")

penguins %>%
  filter(sex == "male") %>%
  group_by(species, island) %>%
  drop_na() %>%
  summarize(max_bill = max(bill_length_mm),
            min_bill = min(bill_length_mm))
```

---

### 5.3 Data Transformation with tidyr

```R
library(tidyr)

employee <- data.frame(
  id = 1:10,
  name = c("John Mendes", "Rob Stewart", "Rachel Abrahamson", "Christy Hickman",
           "Johnson Harper", "Candace Miller", "Carlson Landy", "Pansy Jordan",
           "Darius Berry", "Claudia Garcia"),
  job_title = c("Professional", "Programmer", "Management", "Clerical",
                "Developer", "Programmer", "Management", "Clerical",
                "Developer", "Programmer")
)
```

---

#### Separate One Column into Two

```R
separate(employee, name, into = c("First_Name", "Last_Name"), sep = " ")
```

- Splits one column into multiple based on a separator.

---

#### Unite Two Columns into One

```R
employee <- data.frame(
  id = 1:10,
  first_name = c("John", "Rob", "Rachel", "Christy", "Johnson", "Candace",
                 "Carlson", "Pansy", "Darius", "Claudia"),
  last_name = c("Mendes", "Stewart", "Abrahamson", "Hickman", "Harper", "Miller",
                "Landy", "Jordan", "Berry", "Garcia"),
  job_title = c("Professional", "Programmer", "Management", "Clerical",
                "Developer", "Programmer", "Management", "Clerical",
                "Developer", "Programmer")
)

unite(employee, "name", first_name, last_name, sep = " ")
```

---

#### Mutate for Unit Conversions

```R
penguins %>% mutate(body_mass_kg = body_mass_g / 1000)

penguins %>%
  mutate(
    bill_length_cm = bill_length_mm / 10,
    bill_depth_cm  = bill_depth_mm / 10
  )
```
## Part 6: Data Visualization with ggplot2

---

### 6.1 Basic Setup

```R
library(ggplot2)
library(palmerpenguins)

View(penguins)
```

- `ggplot2` is the main library for creating plots.
- `penguins` dataset is often used for practice.

---

### 6.2 Scatter Plots

```R
ggplot(data = penguins) +
  geom_point(mapping = aes(x = flipper_length_mm, y = body_mass_g))
```

- Plots `flipper_length_mm` on x-axis and `body_mass_g` on y-axis.
- `geom_point()` is used for scatter plots.

---

### 6.3 Aesthetic Mapping

```R
ggplot(penguins) +
  geom_point(aes(x = flipper_length_mm, y = body_mass_g,
                 color = species, shape = species, size = species))

ggplot(penguins) +
  geom_point(aes(x = flipper_length_mm, y = body_mass_g, alpha = species))
```

- Add layers for color, shape, and size.
- `alpha` adjusts transparency.

---

### 6.4 Fixed Aesthetics (Manual Settings)

```R
ggplot(penguins) +
  geom_point(aes(x = flipper_length_mm, y = body_mass_g), color = "purple")
```

---

### 6.5 Line Smoothing

```R
ggplot(penguins) +
  geom_point(aes(x = flipper_length_mm, y = body_mass_g)) +
  geom_smooth(aes(x = flipper_length_mm, y = body_mass_g))

ggplot(penguins) +
  geom_smooth(aes(x = flipper_length_mm, y = body_mass_g, linetype = species))
```

- `geom_smooth()` fits a regression line.

---

### 6.6 Jitter Plot

```R
ggplot(penguins) +
  geom_jitter(aes(x = flipper_length_mm, y = body_mass_g, linetype = species))
```

- Adds small noise to avoid overlap.

---

### 6.7 Bar Charts

```R
ggplot(diamonds) +
  geom_bar(aes(x = cut))

ggplot(diamonds) +
  geom_bar(aes(x = cut, fill = cut)) +
  scale_fill_brewer(palette = "Set2")
```

- Shows count per category of `cut`.
- `fill` colors the bars based on a variable.

---

### 6.8 Faceting (Multiple Plots)

```R
ggplot(penguins) +
  geom_point(aes(x = flipper_length_mm, y = body_mass_g, color = species)) +
  facet_wrap(~ species)

ggplot(penguins) +
  geom_point(aes(x = flipper_length_mm, y = body_mass_g, color = species)) +
  facet_grid(sex ~ species)
```

- `facet_wrap()` creates a grid of plots by a single variable.
- `facet_grid()` creates plots using two variables (rows × columns).

---

### 6.9 Plot Annotations

```R
ggplot(penguins) +
  geom_point(aes(x = flipper_length_mm, y = body_mass_g, color = species)) +
  labs(
    title = "Palmer Penguins: Body Mass vs Flipper Length",
    subtitle = "Sample of 3 Penguin Species",
    caption = "Created by: Pawan Bonde"
  ) +
  annotate("text", x = 220, y = 4200, label = "Gentoos are the largest",
           color = "purple", fontface = "bold", size = 4.5, angle = 30)
```

- `labs()` adds title, subtitle, and caption.
- `annotate()` puts custom text inside the plot.

---

### 6.10 Save Plots

```R
p <- ggplot(penguins) +
  geom_point(aes(x = flipper_length_mm, y = body_mass_g, color = species)) +
  labs(title = "Palmer Penguins")

ggsave("Three_Penguin_Species.png", plot = p)
```

- `ggsave()` saves the most recent or specified plot as an image.

---

### 6.11 Datasaurus Dozen (Fun Visualization)

```R
install.packages("datasauRus")
library(datasauRus)

ggplot(datasaurus_dozen, aes(x = x, y = y, colour = dataset)) +
  geom_point() +
  theme_void() +
  theme(legend.position = "none") +
  facet_wrap(~ dataset, ncol = 3)
```

- Shows how datasets can have similar stats but look very different visually.

---

## Part 7: Bonus Tools — Bias & Statistical Graphics

---

### 7.1 Bias Checking with SimDesign

```R
install.packages("SimDesign")
library(SimDesign)

# Example 1: Temperature Predictions
actual_temp <- c(68.3, 70, 72.4, 4, 71, 67, 69)
predicted_temp <- c(67.9, 69, 71.5, 72, 70, 67, 69)

bias(actual_temp, predicted_temp)

# Example 2: Sales Forecasting
actual_sales <- c(150, 203, 137, 247, 116, 287)
predicted_sales <- c(200, 300, 150, 250, 150, 300)

bias(actual_sales, predicted_sales)
```

- `bias()` calculates systematic error between actual and predicted values.
- Useful for evaluating forecasting models.

---

### 7.2 Statistical Dataset: Anscombe's Quartet

```R
library(Tmisc)
data("quartet")
```

- Contains 4 datasets with the same summary statistics but different distributions.

#### Statistical Summary

```R
quartet %>%
  group_by(set) %>%
  summarise(
    mean_x = mean(x), sd_x = sd(x), min_x = min(x), max_x = max(x),
    mean_y = mean(y), sd_y = sd(y), min_y = min(y), max_y = max(y)
  )
```

---

### 7.3 Visualization of Anscombe’s Quartet

```R
library(ggplot2)

ggplot(quartet, aes(x = x, y = y)) +
  geom_point() +
  geom_smooth(method = "lm", se = FALSE) +
  facet_wrap(~ set)
```

- Shows that visualizing data reveals patterns statistics can't.

---


## 15-04-2025
# Data Manipulation and Transformation in R

## **1. Data Manipulation with dplyr**

The `dplyr` package is a powerful tool for data manipulation in R. It provides a variety of functions to handle and transform data frames.

### **Key Functions in dplyr**
1. **`mutate()`**: Adds new columns or modifies existing ones.
   ```R
   survey %>%
     mutate(Ratio_Hnd = round(Wr.Hnd / NW.Hnd, 2))
   ```
   - Creates a new column `Ratio_Hnd` by dividing `Wr.Hnd` by `NW.Hnd` and rounding to 2 decimal places.

2. **`select()`**: Selects specific columns.
   ```R
   survey %>%
     select(Ratio_Hnd, Clap, Age)
   ```
   - Extracts only the specified columns.

3. **`summarize()`**: Computes summary statistics.
   ```R
   survey %>%
     summarize(mean_of_age = mean(Age, na.rm = TRUE),
               Stan_dev = sd(Age, na.rm = TRUE))
   ```
   - Calculates the mean and standard deviation of `Age`.

4. **`group_by()`**: Groups data by a categorical variable.
   ```R
   survey %>%
     group_by(Sex) %>%
     summarize(mean_of_age = mean(Age, na.rm = TRUE),
               Stan_dev = sd(Age, na.rm = TRUE))
   ```
   - Groups data by `Sex` and computes summary statistics for each group.

---

## **2. Joins in dplyr**

Joins are used to combine data from multiple tables based on common keys. The `dplyr` package provides several types of joins:

### **Types of Joins**
1. **Inner Join**:
   - Combines rows from two tables where the key matches in both.
   ```R
   inner_join(A, B, by = "IdNum")
   ```

2. **Left Join**:
   - Keeps all rows from the left table and matches rows from the right table.
   ```R
   left_join(A, B, by = "IdNum")
   ```

3. **Right Join**:
   - Keeps all rows from the right table and matches rows from the left table.
   ```R
   right_join(A, B, by = "IdNum")
   ```

4. **Full Join**:
   - Combines all rows from both tables, filling in `NA` where there is no match.
   ```R
   full_join(A, B, by = "IdNum")
   ```

### **Example: Joining Course Data**
```R
course_info <- courses %>%
  rename(CourseCode = CourseID) %>%
  inner_join(course_schedule, by = "CourseCode")
```

---

## **3. Data Reshaping with tidyr**

The `tidyr` package is used to reshape data between wide and long formats.

### **Key Functions in tidyr**
1. **`gather()`**: Converts wide data into long format.
   ```R
   gather(comb1, Highlighter, Marker, Pen, Refill, key = "Items", value = "Item_count")
   ```
   - Combines multiple columns (`Highlighter`, `Marker`, etc.) into key-value pairs.

2. **`spread()`**: Converts long data into wide format.
   ```R
   spread(table2, key = 'type', value = 'count')
   ```
   - Spreads key-value pairs into separate columns.

3. **`separate()`**: Splits a column into multiple columns.
   ```R
   separate(table3, rate, into = c("cases", "pop"), sep = "/")
   ```
   - Splits the `rate` column into `cases` and `pop` using `/` as the separator.

4. **`unite()`**: Combines multiple columns into one.
   ```R
   unite(employee, "name", first_name, last_name, sep = " ")
   ```
   - Combines `first_name` and `last_name` into a single column `name`.

---

## **4. Practical Examples**

### **Handling Missing Values**
```R
survey %>%
  summarize(mean_of_age = mean(Age, na.rm = TRUE))
```
- The `na.rm = TRUE` argument ensures missing values are ignored during calculations.

### **Using Backticks for Column Names**
```R
gather(table4a, `1999`, `2000`, key = 'Year', value = 'cases')
```
- Backticks are used for column names that are numbers or contain special characters.

### **Combining Multiple Columns**
```R
gather(comb1, -District, key = "Items", value = "Item_count")
```
- Excludes the `District` column and gathers all other columns.

# Data Joining in R

---

## **1. Joining Course Data**

### **Loading Data**
```R
courses <- read.csv("Courses.csv")
course_schedule <- read.csv("CourseSchedule.csv")
```

### **Inner Join**
```R
course_info <- courses %>%
  rename(CourseCode = CourseID) %>%
  inner_join(course_schedule, by = "CourseCode")

View(course_info)
```

### **Renaming Columns**
```R
rename( < TableName/DataFrame > , < NewName > = < OldName >)
course_schedule <- rename(course_schedule, CourseID = CourseCode)
View(course_schedule)
```

### **Other Joins**
#### Left Join
```R
View(courses %>%
  left_join(course_schedule, by = "CourseID"))
```

#### Right Join
```R
View(courses %>%
  right_join(course_schedule, by = "CourseID"))
```

#### Full Join
```R
View(courses %>%
  full_join(course_schedule, by = "CourseID"))
```

### **Alternative Inner Join**
```R
course_schedule <- rename(course_schedule, CourseID = CourseCode)
crs_2 <- inner_join(course_schedule, courses, by = "CourseID")
```

---

## **2. Joining Order Data**

### **Loading Data**
```R
ord <- read.csv("Orders.csv")
item <- read.csv("Items.csv")
ord_det <- read.csv("Ord_Details.csv")
```

### **Step-by-Step Inner Joins**
1. Join `ord_det` with `ord` on `Order.ID`:
   ```R
   ord_1 <- inner_join(ord_det, ord, by = "Order.ID")
   ```

2. Join the result (`ord_1`) with `item` on `Item.ID`:
   ```R
   ord_2 <- inner_join(ord_1, item, by = "Item.ID")
   View(ord_2)
   ```

### **Chaining Joins with Pipes**
```R
join <- ord %>%
  inner_join(ord_det, by = "Order.ID") %>%
  inner_join(item, by = "Item.ID")
```
## **5. Advanced Topics**

### **Bias Checking with SimDesign**
The `SimDesign` package can calculate bias between actual and predicted values:
```R
bias(actual_temp, predicted_temp)
```
- Computes systematic error between actual and predicted values.

### **Anscombe's Quartet**
Visualizing datasets with identical summary statistics but different distributions:
```R
ggplot(quartet, aes(x = x, y = y)) +
  geom_point() +
  geom_smooth(method = "lm", se = FALSE) +
  facet_wrap(~ set)
```
## **6. Visualization with ggplot2**

### **Scatter Plot**
```R
ggplot(data = penguins) +
  geom_point(mapping = aes(x = flipper_length_mm, y = body_mass_g))
```
- Creates a scatter plot of `flipper_length_mm` vs `body_mass_g`.

### **Faceting**
```R
ggplot(penguins) +
  geom_point(aes(x = flipper_length_mm, y = body_mass_g, color = species)) +
  facet_wrap(~ species)
```
- Creates separate plots for each species.

---

