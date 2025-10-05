# R-for-Excel-User
# Quick Reference Guide

---

## 📚 TABLE OF CONTENTS
1. [Data Loading](#data-loading)
2. [Data Exploration](#data-exploration)
3. [Text Functions](#text-functions)
4. [Logical Functions](#logical-functions)
5. [Date Functions](#date-functions)
6. [Mathematical Functions](#mathematical-functions)
7. [Lookup Functions](#lookup-functions)
8. [Aggregation Functions](#aggregation-functions)
9. [Data Transformation](#data-transformation)
10. [Cleaning Functions](#cleaning-functions)

---

## 1. DATA LOADING {#data-loading}

### `read_csv()`
**What it does:** Reads CSV files into R  
**Package:** `readr`

```r
data <- read_csv("sales.csv")
```
**Expected Output:**
```
# A tibble: 100 × 5
   id    name      date       sales  region
   <dbl> <chr>     <date>     <dbl>  <chr> 
 1     1 Product A 2024-01-15   500  North
 2     2 Product B 2024-01-16   750  South
```

---

### `read_excel()`
**What it does:** Reads Excel files into R  
**Package:** `readxl`

```r
data <- read_excel("sales.xlsx", sheet = "Sales")
```
**Expected Output:** Same as above

---

## 2. DATA EXPLORATION {#data-exploration}

### `head()`
**What it does:** Shows first 6 rows of data  
**Package:** Base R

```r
head(data, 3)  # Show first 3 rows
```
**Expected Output:**
```
  id    name      date       sales
1  1 Product A 2024-01-15   500
2  2 Product B 2024-01-16   750
3  3 Product C 2024-01-17   320
```

---

### `str()`
**What it does:** Shows structure of data (column types, dimensions)  
**Package:** Base R

```r
str(data)
```
**Expected Output:**
```
'data.frame': 100 obs. of 4 variables:
 $ id   : num  1 2 3 4 5 ...
 $ name : chr  "Product A" "Product B" ...
 $ date : Date, format: "2024-01-15" ...
 $ sales: num  500 750 320 ...
```

---

### `summary()`
**What it does:** Provides statistical summary of each column  
**Package:** Base R

```r
summary(data)
```
**Expected Output:**
```
      id            name               date                sales      
 Min.   :  1.0   Length:100        Min.   :2024-01-15   Min.   : 100  
 1st Qu.: 25.8   Class :character  1st Qu.:2024-02-01   1st Qu.: 450  
 Median : 50.5   Mode  :character  Median :2024-02-15   Median : 680  
 Mean   : 50.5                     Mean   :2024-02-15   Mean   : 720  
 3rd Qu.: 75.2                     3rd Qu.:2024-03-01   3rd Qu.:1000  
 Max.   :100.0                     Max.   :2024-03-15   Max.   :2500  
```

---

### `dim()`, `nrow()`, `ncol()`
**What it does:** Shows dimensions of data  
**Package:** Base R

```r
dim(data)    # rows and columns
nrow(data)   # number of rows
ncol(data)   # number of columns
```
**Expected Output:**
```
[1] 100   4    # 100 rows, 4 columns
[1] 100
[1] 4
```

---

### `colnames()`
**What it does:** Shows column names  
**Package:** Base R

```r
colnames(data)
```
**Expected Output:**
```
[1] "id"    "name"  "date"  "sales"
```

---

### `unique()`
**What it does:** Shows unique values in a column  
**Package:** Base R

```r
unique(data$region)
```
**Expected Output:**
```
[1] "North" "South" "East"  "West"
```

---

### `n_distinct()`
**What it does:** Counts unique values  
**Package:** `dplyr`

```r
n_distinct(data$region)
```
**Expected Output:**
```
[1] 4
```

---

### `table()`
**What it does:** Creates frequency table  
**Package:** Base R

```r
table(data$region)
```
**Expected Output:**
```
 East North South  West 
   25    30    20    25
```

---

### `count()`
**What it does:** Counts occurrences, returns dataframe  
**Package:** `dplyr`

```r
data %>% count(region, sort = TRUE)
```
**Expected Output:**
```
  region     n
1  North    30
2   East    25
3   West    25
4  South    20
```

---

## 3. TEXT FUNCTIONS {#text-functions}

### `nchar()`
**What it does:** Counts characters in a string  
**Package:** Base R

```r
data %>% mutate(name_length = nchar(name))
```
**Expected Output:**
```
  id    name        name_length
1  1  Product A          9
2  2  Product B          9
3  3  John                4
```

---

### `str_sub()`
**What it does:** Extracts substring (LEFT, RIGHT, MID)  
**Package:** `stringr`

```r
# LEFT - First 5 characters
data %>% mutate(first_5 = str_sub(name, 1, 5))

# RIGHT - Last 3 characters
data %>% mutate(last_3 = str_sub(name, -3, -1))

# MID - Characters 3 to 7
data %>% mutate(middle = str_sub(name, 3, 7))
```
**Expected Output:**
```
  name          first_5   last_3   middle
1 Product A     Produ     t A      oduct
2 Hello World   Hello     rld      llo W
```

---

### `toupper()`, `tolower()`, `str_to_title()`
**What it does:** Changes text case  
**Package:** Base R / `stringr`

```r
data %>% mutate(
  upper = toupper(name),
  lower = tolower(name),
  title = str_to_title(name)
)
```
**Expected Output:**
```
  name          upper         lower         title
1 hello world   HELLO WORLD   hello world   Hello World
2 PRODUCT a     PRODUCT A     product a     Product A
```

---

### `paste()`, `paste0()`
**What it does:** Concatenates strings  
**Package:** Base R

```r
data %>% mutate(
  full_name = paste(first_name, last_name),
  id_code = paste0("ID_", id)
)
```
**Expected Output:**
```
  first_name  last_name  full_name      id_code
1 John        Doe        John Doe       ID_1
2 Jane        Smith      Jane Smith     ID_2
```

---

### `str_trim()`
**What it does:** Removes leading/trailing whitespace  
**Package:** `stringr`

```r
data %>% mutate(name_clean = str_trim(name))
```
**Expected Output:**
```
  name            name_clean
1 "  John  "      "John"
2 " Product A "   "Product A"
```

---

### `str_replace()`, `str_replace_all()`
**What it does:** Replaces text patterns  
**Package:** `stringr`

```r
data %>% mutate(
  phone_clean = str_replace_all(phone, "[()-\\s]", "")
)
```
**Expected Output:**
```
  phone             phone_clean
1 (555) 123-4567   5551234567
2 555-987-6543     5559876543
```

---

### `separate()`
**What it does:** Splits one column into multiple  
**Package:** `tidyr`

```r
data %>% separate(full_name, into = c("first", "last"), sep = " ")
```
**Expected Output:**
```
  full_name      first   last
1 John Doe       John    Doe
2 Jane Smith     Jane    Smith
```

---

### `str_detect()`
**What it does:** Checks if pattern exists in text  
**Package:** `stringr`

```r
data %>% mutate(has_urgent = str_detect(tolower(description), "urgent"))
```
**Expected Output:**
```
  description              has_urgent
1 Urgent: Fix this        TRUE
2 Normal request          FALSE
3 URGENT action needed    TRUE
```

---

### `str_extract()`
**What it does:** Extracts matching pattern  
**Package:** `stringr`

```r
data %>% mutate(
  numbers = str_extract(text, "\\d+"),
  email_domain = str_extract(email, "@.+")
)
```
**Expected Output:**
```
  text              numbers   email                email_domain
1 Order 12345      12345     john@gmail.com       @gmail.com
2 Item ABC789      789       jane@company.com     @company.com
```

---

## 4. LOGICAL FUNCTIONS {#logical-functions}

### `ifelse()`
**What it does:** Simple if-else logic  
**Package:** Base R

```r
data %>% mutate(
  category = ifelse(sales > 1000, "High", "Low")
)
```
**Expected Output:**
```
  sales   category
1   500   Low
2  1500   High
3   800   Low
```

---

### `case_when()`
**What it does:** Multiple condition logic (nested IF)  
**Package:** `dplyr`

```r
data %>% mutate(
  grade = case_when(
    score >= 90 ~ "A",
    score >= 80 ~ "B",
    score >= 70 ~ "C",
    score >= 60 ~ "D",
    TRUE ~ "F"
  )
)
```
**Expected Output:**
```
  score   grade
1    95   A
2    82   B
3    55   F
4    78   C
```

---

### `is.na()`
**What it does:** Checks for missing values  
**Package:** Base R

```r
# Count NAs per column
colSums(is.na(data))

# Create flag
data %>% mutate(has_missing = is.na(sales))
```
**Expected Output:**
```
id    name  sales
 0      0     15

  sales   has_missing
1   500   FALSE
2    NA   TRUE
3   800   FALSE
```

---

### `replace_na()`
**What it does:** Replaces NA with specified value  
**Package:** `tidyr`

```r
data %>% mutate(sales = replace_na(sales, 0))
```
**Expected Output:**
```
  sales (before)   sales (after)
1       500             500
2        NA               0
3       800             800
```

---

### `coalesce()`
**What it does:** Returns first non-NA value  
**Package:** `dplyr`

```r
data %>% mutate(
  final_price = coalesce(sale_price, regular_price, 0)
)
```
**Expected Output:**
```
  sale_price  regular_price  final_price
1      NA          50            50
2      40          50            40
3      NA          NA             0
```

---

### `filter()`
**What it does:** Filters rows based on conditions  
**Package:** `dplyr`

```r
# Single condition
data %>% filter(sales > 1000)

# Multiple conditions (AND)
data %>% filter(sales > 1000 & region == "North")

# OR condition
data %>% filter(region == "North" | region == "South")
```
**Expected Output:**
```
  id   sales   region
1  5   1500    North
2  8   2000    North
3 12   1200    South
```

---

## 5. DATE FUNCTIONS {#date-functions}

### `ymd()`, `mdy()`, `dmy()`
**What it does:** Converts string to date  
**Package:** `lubridate`

```r
data %>% mutate(
  date1 = ymd("2024-01-15"),      # Year-Month-Day
  date2 = mdy("01/15/2024"),      # Month-Day-Year
  date3 = dmy("15-01-2024")       # Day-Month-Year
)
```
**Expected Output:**
```
  date1       date2       date3
1 2024-01-15  2024-01-15  2024-01-15
```

---

### `year()`, `month()`, `day()`
**What it does:** Extracts date components  
**Package:** `lubridate`

```r
data %>% mutate(
  year = year(date),
  month = month(date),
  month_name = month(date, label = TRUE),
  day = day(date),
  weekday = wday(date, label = TRUE)
)
```
**Expected Output:**
```
  date        year  month  month_name  day  weekday
1 2024-01-15  2024    1      Jan       15   Mon
2 2024-03-22  2024    3      Mar       22   Fri
```

---

### `difftime()`
**What it does:** Calculates difference between dates  
**Package:** Base R

```r
data %>% mutate(
  days_diff = as.numeric(difftime(end_date, start_date, units = "days"))
)
```
**Expected Output:**
```
  start_date  end_date    days_diff
1 2024-01-15  2024-01-20      5
2 2024-02-01  2024-02-15     14
```

---

### Date Arithmetic
**What it does:** Adds/subtracts time periods  
**Package:** `lubridate`

```r
data %>% mutate(
  plus_7_days = date + days(7),
  plus_2_months = date + months(2),
  minus_1_year = date - years(1)
)
```
**Expected Output:**
```
  date        plus_7_days  plus_2_months  minus_1_year
1 2024-01-15  2024-01-22   2024-03-15     2023-01-15
```

---

### `Sys.Date()`, `Sys.time()`
**What it does:** Gets current date/time  
**Package:** Base R

```r
data %>% mutate(
  today = Sys.Date(),
  now = Sys.time()
)
```
**Expected Output:**
```
  today       now
1 2024-10-04  2024-10-04 14:30:25
```

---

## 6. MATHEMATICAL FUNCTIONS {#mathematical-functions}

### `sum()`, `mean()`, `median()`
**What it does:** Basic statistical calculations  
**Package:** Base R

```r
# On single column
sum(data$sales, na.rm = TRUE)
mean(data$sales, na.rm = TRUE)
median(data$sales, na.rm = TRUE)

# In mutate
data %>% mutate(
  total = sum(sales, na.rm = TRUE),
  average = mean(sales, na.rm = TRUE)
)
```
**Expected Output:**
```
[1] 15000      # sum
[1] 750        # mean
[1] 680        # median
```

---

### `round()`, `ceiling()`, `floor()`
**What it does:** Rounds numbers  
**Package:** Base R

```r
data %>% mutate(
  rounded = round(price, 2),
  round_up = ceiling(price),
  round_down = floor(price)
)
```
**Expected Output:**
```
  price    rounded  round_up  round_down
1 15.678   15.68      16         15
2 20.234   20.23      21         20
```

---

### `min()`, `max()`
**What it does:** Finds minimum/maximum values  
**Package:** Base R

```r
min(data$sales, na.rm = TRUE)
max(data$sales, na.rm = TRUE)
```
**Expected Output:**
```
[1] 100    # minimum
[1] 2500   # maximum
```

---

### Basic Math Operators
**What it does:** Performs calculations  
**Package:** Base R

```r
data %>% mutate(
  total = price * quantity,
  discount = price * 0.9,
  tax = price * 1.08,
  profit = revenue - cost,
  percentage = (part / whole) * 100
)
```
**Expected Output:**
```
  price  quantity  total  discount  tax
1   100      5      500      90     108
2    50      3      150      45      54
```

---

### `abs()`, `sqrt()`, `^`
**What it does:** Absolute value, square root, power  
**Package:** Base R

```r
data %>% mutate(
  absolute = abs(value),
  square_root = sqrt(value),
  squared = value^2,
  cubed = value^3
)
```
**Expected Output:**
```
  value  absolute  square_root  squared  cubed
1   -25      25       NA          625    -15625
2    16      16        4          256      4096
3     9       9        3           81       729
```

---

### `%%` (modulo)
**What it does:** Returns remainder of division  
**Package:** Base R

```r
data %>% mutate(
  remainder = id %% 2,
  is_even = ifelse(id %% 2 == 0, "Even", "Odd")
)
```
**Expected Output:**
```
  id  remainder  is_even
1  5      1       Odd
2  8      0       Even
3  11     1       Odd
```

---

## 7. LOOKUP FUNCTIONS {#lookup-functions}

### `left_join()`
**What it does:** VLOOKUP/XLOOKUP equivalent  
**Package:** `dplyr`

```r
# Simple lookup
result <- main_data %>%
  left_join(lookup_table, by = "product_id")

# Different column names
result <- main_data %>%
  left_join(lookup_table, by = c("main_id" = "lookup_id"))
```
**Expected Output:**
```
# main_data                    # lookup_table
  id  product_id                 product_id  product_name  price
1  1      101                       101      Widget A        50
2  2      102                       102      Widget B        75

# result after join
  id  product_id  product_name  price
1  1      101     Widget A        50
2  2      102     Widget B        75
```

---

### `inner_join()`
**What it does:** Keeps only matching rows  
**Package:** `dplyr`

```r
result <- data1 %>% inner_join(data2, by = "id")
```
**Expected Output:**
```
# data1                # data2               # result
  id  name               id  value             id  name     value
1  1  John               1   100               1   John      100
2  2  Jane               3   300               3   Sarah     300
3  3  Sarah              5   500
```

---

### `match()`
**What it does:** INDEX-MATCH equivalent  
**Package:** Base R

```r
data %>% mutate(
  category = category_lookup$name[match(category_id, category_lookup$id)]
)
```
**Expected Output:**
```
  category_id  category
1      1        Electronics
2      3        Clothing
3      2        Food
```

---

## 8. AGGREGATION FUNCTIONS {#aggregation-functions}

### `group_by()` + `summarise()`
**What it does:** Creates pivot tables / aggregations  
**Package:** `dplyr`

```r
# Count by category
data %>%
  group_by(region) %>%
  summarise(count = n())

# Multiple aggregations
data %>%
  group_by(region, category) %>%
  summarise(
    count = n(),
    total_sales = sum(sales, na.rm = TRUE),
    avg_sales = mean(sales, na.rm = TRUE),
    max_sales = max(sales, na.rm = TRUE),
    .groups = "drop"
  )
```
**Expected Output:**
```
  region   count  total_sales  avg_sales  max_sales
1 North      25      15000        600        2500
2 South      20      12000        600        1800
3 East       30      18000        600        2000
```

---

### `pivot_wider()`
**What it does:** Converts long to wide format (Excel pivot)  
**Package:** `tidyr`

```r
data_long %>%
  pivot_wider(
    names_from = month,
    values_from = sales
  )
```
**Expected Output:**
```
# Before (Long)              # After (Wide)
  region  month  sales         region  Jan  Feb  Mar
1 North   Jan     500          North   500  600  700
2 North   Feb     600          South   400  450  500
3 North   Mar     700
4 South   Jan     400
5 South   Feb     450
6 South   Mar     500
```

---

### `pivot_longer()`
**What it does:** Converts wide to long format  
**Package:** `tidyr`

```r
data_wide %>%
  pivot_longer(
    cols = c(Jan, Feb, Mar),
    names_to = "month",
    values_to = "sales"
  )
```
**Expected Output:** (Reverses the wide format back to long)

---

## 9. DATA TRANSFORMATION {#data-transformation}

### `mutate()`
**What it does:** Creates or modifies columns  
**Package:** `dplyr`

```r
data %>% mutate(
  new_column = price * quantity,
  category = toupper(category)
)
```
**Expected Output:**
```
  price  quantity  new_column  category
1   50      5         250      ELECTRONICS
2   30      2          60      CLOTHING
```

---

### `select()`
**What it does:** Selects specific columns  
**Package:** `dplyr`

```r
# Select specific columns
data %>% select(name, sales, region)

# Remove columns
data %>% select(-id, -date)

# Select by pattern
data %>% select(starts_with("sale"))
data %>% select(contains("price"))
```
**Expected Output:**
```
  name      sales  region
1 Product A  500   North
2 Product B  750   South
```

---

### `arrange()`
**What it does:** Sorts data  
**Package:** `dplyr`

```r
# Ascending
data %>% arrange(sales)

# Descending
data %>% arrange(desc(sales))

# Multiple columns
data %>% arrange(region, desc(sales))
```
**Expected Output:**
```
  id   sales  region
1  3    100   East
2  1    500   North
3  5   1500   North
4  2   2000   South
```

---

### `distinct()`
**What it does:** Removes duplicate rows  
**Package:** `dplyr`

```r
# Remove all duplicates
data %>% distinct()

# Keep first occurrence based on specific columns
data %>% distinct(product_id, .keep_all = TRUE)
```
**Expected Output:**
```
# Before                    # After
  id  product                id  product
1  1  Widget A               1  Widget A
2  2  Widget B               2  Widget B
3  3  Widget A               4  Widget C
4  4  Widget C
```

---

### `across()`
**What it does:** Applies function to multiple columns  
**Package:** `dplyr`

```r
# Apply to specific columns
data %>%
  mutate(across(c(price, cost), round, 2))

# Apply to all numeric columns
data %>%
  mutate(across(where(is.numeric), ~. * 1.1))

# Apply to all character columns
data %>%
  mutate(across(where(is.character), str_trim))
```
**Expected Output:**
```
  price  cost
1 15.68  10.23
2 20.99  15.45
```

---

### `rowwise()`
**What it does:** Performs row-wise calculations  
**Package:** `dplyr`

```r
data %>%
  rowwise() %>%
  mutate(
    row_total = sum(c_across(Jan:Dec), na.rm = TRUE),
    row_avg = mean(c_across(Jan:Dec), na.rm = TRUE)
  ) %>%
  ungroup()
```
**Expected Output:**
```
  Jan  Feb  Mar  row_total  row_avg
1 100  150  200    450        150
2 200  250  300    750        250
```

---

## 10. CLEANING FUNCTIONS {#cleaning-functions}

### `clean_names()`
**What it does:** Standardizes column names  
**Package:** `janitor`

```r
data %>% clean_names()
```
**Expected Output:**
```
# Before                    # After
  Product Name               product_name
  Sales Amount               sales_amount
  Date Created               date_created
```

---

### `na.omit()`
**What it does:** Removes rows with any NA  
**Package:** Base R

```r
data_clean <- na.omit(data)
```
**Expected Output:**
```
# Before (5 rows)           # After (3 rows, removed 2 with NA)
  id  name    sales           id  name    sales
1  1  John     500            1   John     500
2  2  Jane      NA            3   Bob      800
3  3  Bob      800            5   Alice    600
4  4  Sara      NA
5  5  Alice    600
```

---

### `duplicated()`
**What it does:** Identifies duplicate rows  
**Package:** Base R

```r
# Find duplicates
data %>% filter(duplicated(.) | duplicated(., fromLast = TRUE))

# Remove duplicates
data %>% filter(!duplicated(.))
```
**Expected Output:**
```
  id  product    is_duplicate
1  1  Widget A      FALSE
2  2  Widget B      FALSE
3  3  Widget A      TRUE   (duplicate)
```

---

### Complete Cleaning Pipeline Example

```r
cleaned_data <- raw_data %>%
  # Step 1: Clean column names
  clean_names() %>%
  
  # Step 2: Trim whitespace from text columns
  mutate(across(where(is.character), str_trim)) %>%
  
  # Step 3: Convert empty strings to NA
  mutate(across(where(is.character), ~na_if(., ""))) %>%
  
  # Step 4: Remove rows that are all NA
  filter(!if_all(everything(), is.na)) %>%
  
  # Step 5: Remove duplicate rows
  distinct() %>%
  
  # Step 6: Convert data types
  mutate(
    date = ymd(date),
    amount = as.numeric(amount)
  ) %>%
  
  # Step 7: Handle missing values
  mutate(
    sales = replace_na(sales, 0),
    region = replace_na(region, "Unknown")
  )
```

---

## 📝 PRACTICE EXERCISES

### Exercise 1: Text Manipulation
```r
# Given this data:
data <- data.frame(
  name = c("  john DOE  ", "JANE smith", "bob JONES"),
  email = c("JOHN@EMAIL.COM", "jane@email.com", "BOB@EMAIL.COM")
)

# Task: Clean names (trim, title case) and emails (lowercase)
# Your solution here:
```

### Exercise 2: Aggregation
```r
# Given sales data with columns: region, product, sales
# Task: Create a summary showing total sales by region
# Your solution here:
```

### Exercise 3: Date Calculations
```r
# Given data with order_date
# Task: Add columns for year, month, and days_since_order
# Your solution here:
```

---

## 🎯 QUICK TIPS

1. **Always use `na.rm = TRUE`** in mathematical functions when you have missing values
2. **Use pipes `%>%`** to chain operations for readability
3. **`across()` is your friend** for applying functions to multiple columns
4. **`case_when()` is better than nested `ifelse()`**
5. **Always `ungroup()`** after using `rowwise()` or `group_by()`
6. **Use `glimpse()` instead of `str()`** for better readability with tidyverse

---

## 📦 ESSENTIAL PACKAGES TO LOAD

```r
library(dplyr)      # Data manipulation
library(tidyr)      # Data tidying
library(stringr)    # String operations
library(lubridate)  # Date operations
library(readr)      # Reading CSV files
library(readxl)     # Reading Excel files
library(janitor)    # Data cleaning
```

---

**Happy Learning! 🚀**
Practice these functions daily to master R data analysis!
