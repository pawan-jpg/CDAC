# A Beginner's Deep Dive into R Programming

Welcome to your detailed guide to the R programming language! This document expands on the notes you provided, breaking down each concept with in-depth explanations, code examples, and valuable resources to accelerate your learning.

## 1. Introduction to R

### History and Purpose

R was created by **Ross Ihaka** and **Robert Gentleman** at the University of Auckland, New Zealand, and is currently developed by the R Development Core Team. The project was conceived in 1992, with an initial version released in 1993.

As your notes mention, R was inspired by the **S language**, which was developed at Bell Laboratories for statistical computing. The name "R" is partly a play on the names of the two first authors and partly a nod to its predecessor, S.

The primary goal of R is to provide a powerful and flexible environment for **statistical analysis and data visualization**. It's the language of choice for statisticians, data scientists, and researchers in many fields.

### Key Features of R

- **Specialized for Statistics:** While you can perform general programming tasks, R's true power lies in its vast ecosystem for statistics, data manipulation, and machine learning.
    
- **General Programming Constructs:** It includes all the standard features you'd expect from a programming language: variables, loops, conditional statements (`if`/`else`), and functions.
    
- **Interpreted (Scripting) Language:** R is an interpreted language, which means you can run code line-by-line without needing a compilation step. This makes it excellent for interactive data exploration. There is no `main()` function to serve as an entry point; you can just start writing and executing your script from the first line.
    
- **Powerful Packages:** R's greatest strength is its package ecosystem, available primarily through the **Comprehensive R Archive Network (CRAN)**. These packages are collections of functions, data, and compiled code that extend R's capabilities.
    
    - **`ggplot2`:** A world-class package for creating elegant and complex data visualizations.
        
    - **`dplyr` and `data.table`:** Packages for high-performance data manipulation.
        
    - **`caret` and `tidymodels`:** Frameworks for machine learning.
        

### Internet Resources

- **The R Project for Statistical Computing:** The official homepage for R.
    
    - [https://www.r-project.org/](https://www.r-project.org/ "null")
        
- **What is R? - An Introduction:** A great starting point from the R project itself.
    
    - [https://www.r-project.org/about.html](https://www.r-project.org/about.html "null")
        

## 2. Getting Started

### Environment Setup

To start using R, you need two things:

1. **The R Language Itself:** This is the core engine. You must install this first.
    
    - **Download Link:** [https://cran.r-project.org/](https://cran.r-project.org/ "null") (This is the homepage for CRAN, where you can find download links for Windows, macOS, and Linux).
        
2. **An Integrated Development Environment (IDE):** An IDE makes writing and managing your code much easier. While you can use a simple text editor, **RStudio** is the industry standard and is highly recommended for beginners.
    
    - **RStudio Desktop:** It includes a console, a script editor, and tools for plotting, package management, and debugging.
        
    - **Download Link:** [https://posit.co/download/rstudio-desktop/](https://posit.co/download/rstudio-desktop/ "null")
        

> **Note:** Always install R _before_ you install RStudio. RStudio is just a user interface that needs the underlying R engine to function.

### Executing R Code

You can run R code in a few different ways:

1. **Interactive Console:**
    
    - Open RStudio or type `R` in your computer's terminal. You'll see a `>` prompt.
        
    - You can type commands here and press `Enter` to execute them immediately. This is great for quick tests and exploration.
        
    
    ```
    > 5 + 10
    [1] 15
    > print("Hello, World!")
    [1] "Hello, World!"
    ```
    
2. **R Script File:**
    
    - For any code you want to save and reuse, you should write it in a script file with a `.R` extension (e.g., `my_analysis.R`).
        
    - In RStudio, you can open a new script with `File -> New File -> R Script` (or `Ctrl + Shift + N`).
        
    - You can run lines directly from the script editor to the console using `Ctrl + Enter`.
        
    - To run the entire script from your computer's terminal, you use the `Rscript` command:
        
    
    ```
    Rscript my_analysis.R
    ```
    

### RStudio Shortcuts (as mentioned in your notes)

- `Ctrl + L`: Clears the console.
    
- `Ctrl + Shift + N`: Creates a new, empty R script.
    
- `Ctrl + Enter`: Runs the current line or the selected block of code from the script editor.
    
- `Ctrl + Shift + S`: Runs the entire script that is currently open in the editor.
    
- `?function_name`: Shows the help documentation for a function in the Help panel (e.g., `?print`).
    
- `?? "topic"`: Searches the help documentation for a specific topic or keyword (e.g., `?? "linear regression"`).
    

## 3. The Building Blocks: Identifiers & Variables

### Identifiers

An identifier is the name you give to an entity in R, like a variable or a function.

**Rules for Naming Identifiers:**

- Can contain letters, numbers, the dot (`.`), and the underscore (`_`).
    
- **Must not** start with a number (e.g., `1st_variable` is invalid).
    
- **Must not** start with an underscore (e.g., `_my_var` is invalid).
    
- **Cannot** contain special characters like `!`, `@`, `$`, `+`, `-`, `*`, `/`.
    
- R is case-sensitive: `my_variable` is different from `My_Variable`.
    

**Naming Conventions:** While R allows for flexibility, the community has adopted a few common styles. The most prominent style guide is the **Tidyverse style guide**.

- **`snake_case`:** All lowercase with words separated by underscores. This is generally preferred. (e.g., `first_name`, `monthly_sales`).
    
- **`dot.case`:** Using dots to separate words. This is also very common in R, but can sometimes be confused with S3 methods (an object-oriented concept in R). (e.g., `first.name`).
    

```
# Valid identifiers
my_variable <- 10
results.2023 <- 500
student_age <- 25

# Invalid identifiers
# 1st_place <- "Gold"  # Starts with a number
# student-name <- "John" # Contains a hyphen
# _private_var <- TRUE # Starts with an underscore
```

### Variables and Assignment

A variable is a named storage location in memory. You assign a value to a variable using an **assignment operator**.

**Assignment Operators:** Your notes show several ways to assign variables.

- `<-` (The "gets" operator): This is the most common and **idiomatic** assignment operator in R. It's the preferred operator by the R community.
    
- ` = ` (Equals sign): This also works for assignment, but it's generally reserved for setting arguments inside function calls. To avoid confusion, it's best to use `<-` for variable assignment.
    
- `->` (Rightward assignment): You can use this to assign in the other direction. It's rarely used but is valid.
    

```
# Preferred method
x <- 100

# Also works, but less common for assignment
y = 200

# Rightward assignment (rarely used)
300 -> z

# Print the values
print(x)
print(y)
print(z)
```

**Printing Variables:**

- `print(variable)`: The standard function to display a value.
    
- `cat(...)`: "Concatenate and print". This function gives you more control over the output, allowing you to join multiple items together. It does not automatically add a newline at the end.
    
- `paste(...)`: A function to concatenate strings together.
    

```
num <- 100
print(num)
# [1] 100

# cat() joins the items. Note the space we have to add manually.
cat("The value of num is", num, "\n") # \n adds a new line
# The value of num is 100

# paste() creates a single string, which you can then print
message <- paste("The value of num is", num)
print(message)
# [1] "The value of num is 100"
```

### Special Pre-defined Values

- `NA`: **Not Available**. This is R's way of representing missing or undefined values. It's a placeholder.
    
- `Inf`: **Infinity**. This represents an infinitely large number, often the result of dividing by zero. (`-Inf` is negative infinity).
    
- `NaN`: **Not a Number**. This is the result of an undefined mathematical operation, like `0/0`.
    

### Internet Resources

- **Google's R Style Guide:** A good reference for coding conventions.
    
    - [https://google.github.io/styleguide/Rguide.html](https://google.github.io/styleguide/Rguide.html "null")
        
- **The `<- `  vs ` = ` Assignment Operator in R:** An article explaining the subtle differences.
    
    - [https://www.statology.org/arrow-vs-equal-sign-in-r/](https://www.google.com/search?q=https://www.statology.org/arrow-vs-equal-sign-in-r/ "null")
        

## 4. Core Data Structures in R

In R, the "data type" often refers to the atomic type of the elements within a data structure. The main atomic types are `numeric`, `integer`, `character`, `logical`, `complex`, and `raw`. These are stored in various data structures.

### 4.1. Vectors

A vector is the most fundamental data structure in R. It's a one-dimensional sequence of data elements of the **same atomic type**.

**Creating Vectors:** You create vectors using the `c()` function, which stands for "combine" or "concatenate".

- **`numeric`**: The default type for numbers, which includes decimals.
    
- **`integer`**: For whole numbers. You must use an `L` suffix to explicitly create an integer.
    
- **`character`**: For text strings (can use `"` or `'`).
    
- **`logical`**: For `TRUE` or `FALSE` values (can be abbreviated to `T` or `F`).
    

```
# A numeric vector
salaries <- c(50000, 65000.50, 120000)

# An integer vector
ages <- c(30L, 45L, 22L)

# A character vector
names <- c("Alice", "Bob", "Charlie")

# A logical vector
is_manager <- c(TRUE, FALSE, TRUE)
```

**Type Coercion in Vectors:** If you try to combine different atomic types in a single vector, R will **coerce** them to the most flexible type to maintain homogeneity. The hierarchy is: `logical` -> `integer` -> `numeric` -> `character`.

```
# The numeric 10 is coerced to a character string "10"
mixed_vector <- c("Alice", 10, TRUE)
print(mixed_vector)
# [1] "Alice" "10"    "TRUE"

# The result is a character vector
class(mixed_vector)
# [1] "character"
```

**Vector Indexing:**

- **1-Based:** Unlike many other languages (Python, Java), R's indexing starts at **1**.
    
- **Positive Indexing:** Use positive numbers to get elements at specific positions.
    
- **Negative Indexing:** Use negative numbers to **exclude** elements at specific positions.
    

```
numbers <- c(10, 20, 30, 40, 50, 60, 70, 80, 90, 100)

# Get the first element
numbers[1] # Returns 10

# Get the last element (length() gets the size of the vector)
numbers[length(numbers)] # Returns 100

# Get elements at positions 2, 4, and 6
numbers[c(2, 4, 6)] # Returns 20 40 60

# Get all elements EXCEPT the first one
numbers[-1] # Returns 20 30 40 50 60 70 80 90 100

# Get all elements EXCEPT those at positions 2, 4, and 6
numbers[c(-2, -4, -6)] # Returns 10 30 50 70 80 90 100
```

**Slicing (Subsetting a Range):** You can select a contiguous block of elements using the colon operator `:`.

```
numbers <- c(10, 20, 30, 40, 50, 60, 70, 80, 90, 100)

# Get elements from the 3rd to the 7th position
numbers[3:7] # Returns 30 40 50 60 70
```

**Vectorization (The "Broadcast Operator"):** This is a core concept in R. It means that operations are automatically applied to each element of a vector without needing to write an explicit loop. This makes code more concise and much faster.

```
numbers_1 <- c(10, 20, 30)
numbers_2 <- c(2, 4, 6)

# Add 5 to every element in numbers_1
result_add <- numbers_1 + 5 # Returns 15 25 35
print(result_add)

# Multiply each element of numbers_1 by the corresponding element of numbers_2
result_mult <- numbers_1 * numbers_2 # Returns 20 80 180
print(result_mult)

# Logical operations are also vectorized
logical_vec <- c(TRUE, FALSE, TRUE)
!logical_vec # Returns FALSE TRUE FALSE
```

### 4.2. Lists

A list is a more flexible one-dimensional collection that can hold elements of **different types**. Each element of a list can be a vector, another list, a data frame, a function, etc.

**Creating Lists:**

```
# A list containing a character vector, a numeric vector, and a single logical value
my_list <- list(
  c("apple", "orange"),
  c(1, 2, 3, 4, 5),
  TRUE
)

# You can also name the elements of a list, which is highly recommended
employee <- list(
  name = "John Doe",
  age = 42L,
  salary = 95000,
  is_remote = TRUE,
  projects = c("Project A", "Project C")
)
```

**Accessing List Elements:** There are three main ways to access elements in a list:

- `[[index]]` or `[[ "name" ]]`: Extracts a **single element** from the list. The object returned will have the type of the element itself (e.g., a vector).
    
- `[index]` or `[ "name" ]`: **Subsets the list**. This always returns another **list**, even if you only select one element.
    
- `$name`: A convenient shortcut to access a named element using `[[ "name" ]]`.
    

```
# Using the 'employee' list from above

# Extract the 'name' element. The result is a character vector of length 1.
employee[["name"]] # Returns "John Doe"
employee$name      # Same as above, and more common

# Extract the second element ('age'). The result is an integer vector.
employee[[2]] # Returns 42

# Get the second project from the 'projects' vector within the list
employee$projects[2] # Returns "Project C"
employee[[5]][2]     # Same as above

# -- Difference between [ ] and [[ ]] --

# [[1]] returns the element itself (a character vector)
class(my_list[[1]]) # "character"

# [1] returns a new list containing that element
class(my_list[1]) # "list"
```

### 4.3. Matrices

A matrix is a **two-dimensional** collection of data elements of the **same atomic type**. It's organized into a fixed number of rows and columns.

**Creating Matrices:** You use the `matrix()` function.

- `data`: The vector of elements to fill the matrix.
    
- `nrow`: The number of rows.
    
- `ncol`: The number of columns.
    
- `byrow`: A logical value. If `FALSE` (the default), the matrix is filled column-by-column. If `TRUE`, it's filled row-by-row.
    

```
# Filled by column (default)
m1 <- matrix(c(10, 20, 30, 40, 50, 60), nrow = 2, ncol = 3)
#      [,1] [,2] [,3]
# [1,]   10   30   50
# [2,]   20   40   60
print(m1)

# Filled by row
m2 <- matrix(c(10, 20, 30, 40, 50, 60), nrow = 2, ncol = 3, byrow = TRUE)
#      [,1] [,2] [,3]
# [1,]   10   20   30
# [2,]   40   50   60
print(m2)
```

**Naming and Accessing Matrix Elements:** You can assign names to the rows and columns using `dimnames` to make subsetting easier. You access elements using the `[row, column]` syntax.

```
# Define row and column names
row_names <- c("Sale1", "Sale2")
col_names <- c("ProductA", "ProductB", "ProductC")

# Create a named matrix
sales_data <- matrix(
  c(100, 150, 200, 220, 90, 180),
  nrow = 2,
  byrow = TRUE,
  dimnames = list(row_names, col_names)
)
print(sales_data)

# Get the value at the first row, third column
sales_data[1, 3] # Returns 200

# Get the value for "Sale2" and "ProductA"
sales_data["Sale2", "ProductA"] # Returns 220

# Get the entire first row (leave the column part blank)
sales_data[1, ] # Returns the vector for ProductA, ProductB, ProductC for Sale1

# Get the entire third column (leave the row part blank)
sales_data[, 3] # Returns the vector for ProductC sales
```

### 4.4. Factors

A factor is a special type of vector used to store **categorical data**. R stores this data more efficiently by assigning an integer to each unique category (called a "level"). This is very useful for statistical modeling and plotting.

```R
# A vector of categorical data
	directions <- c("North", "East", "South", "East", "North", "North")

# Convert it to a factor
factor_directions <- factor(directions)

print(factor_directions)
# [1] North East  South East  North North
# Levels: East North South  (Levels are stored alphabetically by default)

# You can see how R stores it internally
str(factor_directions)
# Factor w/ 3 levels "East","North",..: 2 1 3 1 2 2

# You can control the order of the levels
sizes <- c("Small", "Large", "Medium", "Small", "Large")
factor_sizes <- factor(sizes, levels = c("Small", "Medium", "Large"), ordered = TRUE)
print(factor_sizes)
# [1] Small  Large  Medium Small  Large
# Levels: Small < Medium < Large
```

### 4.5. Data Frames

The **data frame** is the most important data structure for data analysis in R. It's a two-dimensional, table-like structure where:

- Columns can have different atomic types (e.g., one column character, another numeric).
    
- Each column must have the same number of elements.
    
- It's essentially a `list` of equal-length `vectors`.
    

**Creating Data Frames:**

```r
# Create vectors for our columns
names <- c("Alice", "Bob", "Charlie", "David")
ages <- c(25, 32, 28, 45)
salaries <- c(68000, 89000, 75000, 112000)
is_full_time <- c(TRUE, TRUE, FALSE, TRUE)

# Create the data frame
employees_df <- data.frame(
  name = names,
  age = ages,
  salary = salaries,
  full_time = is_full_time
)

print(employees_df)
#      name age salary full_time
# 1   Alice  25  68000      TRUE
# 2     Bob  32  89000      TRUE
# 3 Charlie  28  75000     FALSE
# 4   David  45 112000      TRUE
```

**Accessing Data Frame Elements:** You can access data frame elements like a `matrix` (`[row, column]`) or like a `list` (`$` or `[[ ]]`).

```R
# Get the 'salary' column (returns a vector)
employees_df$salary
employees_df[["salary"]]

# Get the first row
employees_df[1, ]

# Get the value in the 3rd row, 2nd column (age of Charlie)
employees_df[3, 2] # Returns 28

# Get the name and salary columns only
employees_df[ , c("name", "salary")]
```

### Internet Resources

- **R for Data Science (R4DS) by Hadley Wickham:** The definitive, free online book for learning modern R.
    
    - **Vectors:** [https://r4ds.had.co.nz/vectors.html](https://r4ds.had.co.nz/vectors.html "null")
        
    - **Data Frames (Tibbles):** [https://r4ds.had.co.nz/tibbles.html](https://r4ds.had.co.nz/tibbles.html "null") (Tibbles are the modern version of data frames).
        
- **R-bloggers:** A central hub for R news and tutorials from hundreds of contributors.
    
    - [https://www.r-bloggers.com/](https://www.r-bloggers.com/ "null")
        

## 5. Inspecting and Managing Your Data

R provides many useful functions to understand the structure and content of your data objects.

- `class(object)`: Tells you the high-level data structure (e.g., "data.frame", "matrix", "numeric").
    
- `typeof(object)`: Tells you the underlying atomic type (e.g., "double", "integer", "list").
    
- `str(object)`: **Structure**. This is one of the most useful functions. It gives a compact summary of any R object, including the number of observations/variables, column names, data types, and a preview of the data.
    
- `summary(object)`: Provides a statistical summary. For numeric columns, it gives min, max, mean, median, and quartiles. For factor/character columns, it gives counts.
    
- `head(object, n)`: Shows the first `n` rows (default is 6).
    
- `tail(object, n)`: Shows the last `n` rows (default is 6).
    
- `names(object)` or `colnames(object)`: Gets or sets the column names.
    
- `rownames(object)`: Gets or sets the row names.
    
- `nrow(object)`: Returns the number of rows.
    
- `ncol(object)`: Returns the number of columns.
    
- `dim(object)`: Returns the dimensions (rows and columns) as a vector.
    

```
# Using the employees_df from before
str(employees_df)
# 'data.frame':	4 obs. of  4 variables:
#  $ name     : chr  "Alice" "Bob" "Charlie" "David"
#  $ age      : num  25 32 28 45
#  $ salary   : num  68000 89000 75000 112000
#  $ full_time: logi  TRUE TRUE FALSE TRUE

summary(employees_df)
#      name                age            salary         full_time
#  Length:4           Min.   :25.00   Min.   : 68000   Mode :logical
#  Class :character   1st Qu.:27.25   1st Qu.: 73250   FALSE:1
#  Mode  :character   Median :30.00   Median : 82000   TRUE :3
#                     Mean   :32.50   Mean   : 86000
#                     3rd Qu.:35.25   3rd Qu.: 94750
#                     Max.   :45.00   Max.   :112000

head(employees_df, 2)
#    name age salary full_time
# 1 Alice  25  68000      TRUE
# 2   Bob  32  89000      TRUE
```

### Type Inspection and Conversion

- **`is.*()` functions**: These functions check if an object is of a certain type and return `TRUE` or `FALSE`. Examples: `is.numeric()`, `is.character()`, `is.data.frame()`.
    
- **`as.*()` functions**: These functions attempt to convert an object from one type to another. Examples: `as.numeric()`, `as.character()`, `as.data.frame()`.
    

```
x <- "123.45"
is.character(x) # TRUE
is.numeric(x)   # FALSE

# Convert to numeric
y <- as.numeric(x)
is.numeric(y)   # TRUE
print(y)        # 123.45

# Logical conversion
as.logical(1)   # TRUE
as.logical(0)   # FALSE
as.logical(100) # TRUE (any non-zero number is TRUE)
```

## 6. Programming Fundamentals

### Loops

While vectorization is preferred for speed, sometimes you need to use a loop. The `for` loop is the most common.

**Syntax:**

```
for (variable in sequence) {
  # Code to execute for each item in the sequence
}
```

**Example:**

```
# Loop through a vector of numbers
numbers <- c(10, 20, 30, 40, 50)
for (value in numbers) {
  cat("The current value is:", value, "\n")
}

# Loop from 1 to 5
for (i in 1:5) {
  print(paste("This is iteration number", i))
}
```

### Functions

Functions are reusable blocks of code.

**Custom (User-Defined) Functions:** You define a function using the `function()` keyword.

```
# Function declaration
# This function takes two arguments and returns their sum
add_numbers <- function(x, y) {
  result <- x + y
  return(result) # The 'return()' is optional for the last expression
}

# Function call
sum_result <- add_numbers(10, 5)
print(sum_result) # 15

# A function without arguments
say_hello <- function() {
  print("Hello from a function!")
}
say_hello()
```

**Important Built-in Functions:**

- `getwd()`: **Get Working Directory**. Shows the default folder path where R will look for files you want to read and save files you create.
    
- `setwd("path/to/your/directory")`: **Set Working Directory**. Changes the default folder path.
    
- `install.packages("package_name")`: Downloads and installs a package from CRAN. You only need to do this once per package.
    
- `library(package_name)`: Loads an installed package into your current R session, making its functions available to use. You must do this every time you start a new R session.
    

```
# See where R is currently working
getwd()

# Install the 'dplyr' package for data manipulation (only need to run once)
# install.packages("dplyr")

# Load the dplyr library for use in this session
library(dplyr)

# Now you can use functions from dplyr, like glimpse() which is similar to str()
glimpse(employees_df)
```

## 7. Test Your Knowledge

Let's test what you've learned with a few questions.

**Question 1:** What will be the output of the following code? `my_vector <- c(10, "20", TRUE)` `class(my_vector)`

A) `numeric` B) `list` C) `character` D) It will cause an error.

<details> <summary>Click to see the answer</summary> <br> **Answer: C) `character`** <br><br> **Explanation:** When you combine different data types into a single vector, R uses type coercion to make all elements the same type. The coercion hierarchy is `logical` -> `integer` -> `numeric` -> `character`. Since `&quot;20&quot;` is a character string, the number `10` and the logical `TRUE` are both converted to character strings, resulting in a character vector. </details>

<br>

**Question 2:** In R, how do you select the element at the 3rd row and 5th column of a data frame named `df`?

A) `df[5, 3]` B) `df[3, 5]` C) `df(3, 5)` D) `df[[3]][[5]]`

<details> <summary>Click to see the answer</summary> <br> **Answer: B) `df[3, 5]`** <br><br> **Explanation:** R uses the syntax `[row, column]` for indexing 2D structures like matrices and data frames. The row index always comes first. </details>

<br>

**Question 3:** What is the primary difference between accessing a list element with `my_list[1]` versus `my_list[[1]]`?

A) There is no difference; they do the same thing. B) `[1]` gets the name of the element, while `[[1]]` gets the value. C) `[1]` returns a new list containing the first element, while `[[1]]` extracts the first element itself. D) `[1]` is for numeric lists, and `[[1]]` is for character lists.

<details> <summary>Click to see the answer</summary> <br> **Answer: C) `[1]` returns a new list containing the first element, while `[[1]]` extracts the first element itself.** <br><br> **Explanation:** Single brackets `[]` are for **subsetting** and always return an object of the same class as the original. Double brackets `[[ ]]` are for **extracting** a single element, and the class of the returned object will be the class of the element inside the list. </details>

<br>

**Question 4:** You want to install the popular data visualization package `ggplot2` and use it in your script. Which two commands do you need, and in what order?

A) `library(ggplot2)` then `install.packages("ggplot2")` B) `require(ggplot2)` then `load("ggplot2")` C) `install.packages("ggplot2")` then `library(ggplot2)` D) `run("ggplot2")` then `attach("ggplot2")`

<details> <summary>Click to see the answer</summary> <br> **Answer: C) `install.packages(&quot;ggplot2&quot;)` then `library(ggplot2)`** <br><br> **Explanation:** You must first **install** the package to download it from the internet onto your computer. This only needs to be done once. Then, in every new R session where you want to use it, you must **load** it into memory with the `library()` command. </details>
