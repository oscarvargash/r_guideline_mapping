# What is R?

R is a coding language with free distribution, designed to perform statistical analyses. R has a basic library of functions (addition / substraction), but an array of packages are available to perform a wide range of functions, including map making.

## Why using R

Because R is code-based, "scripts" (lines of of code to perform analyses) can easily be saved and rerun. For example, if you add data to a dataset, you can rerun your script and obtain an updated answer.

# Setting up r in your computer
 
R is free, go to (https://www.r-project.org/) click on [download R] and select the link to [Oregon State University], which is the closest to California. Follow the steps and open R.

# Working with R

When you open R you will get a interactive window like this, also called the R console

![](img/r_window.png)

You can write in this window and interact wih R directly. Let's created a variable "x" and asing the nuber "2" to it. Theype the floowing text and press enter.

```
x <- 2
```
Now when you type `x` in the console, it will return 2.
```
x
```

Because we want to create code that we can re-run, it is better to create a text document that we can execute in R. In order to do so, click on the white page icon at the top that when hover over says "Create, a new empty document on the editor". In the new windown type:
```
x <- 2
y <- x + x
y
```
The editor with the code should look like this:
![](img/editor.png)

MAC: to run the code select it and press <kbd>command</kbd> + <kbd>return</kbd>
PC: to run the code selct it and press <kbd>control</kbd> + <kbd>r</kbd>
R is now making and addition with x, the returning value is then assigned to y. 

You can save the text file with the code by going to "file" in the menu and then "save". Save your script in any location you want

Congratulations, you have created your first script!

## Packages

Packages are group of functions that can be installed in your R environment. Dplyr is a great package for managing tables.

To install the package we neet to type:

```
install.packages("dplyr")
```

Now that the package is installed we need to load the package in R by typing

```
library(dplyr)
```
## Importing data and doing some basic cleaning

The power of R relies on importig datasets and easily managing it. We will import to R a dataset of Darlingtonia califonica download from GBIF.

You can download the dataset from this link

https://www.dropbox.com/s/u3xl7v1nws5xzjx/daca.csv?dl=0

Now let's open R and create a new document, save the document as "test2.r"

We will need to load dplyr which is a useful package for managing data stored as a table

```
library(dplyr)
```

Now we need to import the data. Notice that the file that you download "daca.csv". We can do that by typing:
```
data <- read.csv("daca.csv")
```
The data has more than a thousand ocurrences, we can check that the top part of the table by"
```
head(data)
```
We are interested in mapping the ocurrences, so therefore one of the columns that are of our interest is "decimalLatitude". We can see all the values in that column by typing:
```
data$decimalLatitude
```

You will notice that there is a lot of missing data stored in the table as "NA". We can remove all the entries that do not contain data from the table using dplyr.
```
geodata <- data %>% filter(!is.na(decimalLatitude))
head(geodata)
```

Notice that we are creating a second dataframe called "geodata". Now let's check that the "decimalLatitude" have only values.
```
geodata$decimalLatitude
```
now that our data looks filtered, we can plot the data to see how it looks. This is easily done by typing:
```
plot(geodata$decimalLatitude)
```

A histogram in this case will be more useful:
```
hist(geodata$decimalLatitude)
```

We can change the breaks in our histogram easily by:
```
hist(geodata$decimalLatitude, breaks = 60)
```

We see that there are some values extreme and seem outliers. We can remove extreme values filtering out values outside the 0.5%  and in the 99.5% percentiles. We can see those values by typing:
```
quantile(geodata$decimalLatitude, c(0.005, 0.995))
```

With the ouput values we re-filter our dataset removing outliers:

```
geodata_f <- geodata %>% filter(decimalLatitude > 37, decimalLatitude < 46) 
```

Again, notice that we created a third dataframe. Let's check that our filtering ourked out.
```
plot(geodata_f$decimalLatitude)
hist(geodata_f$decimalLatitude)
```

So latitude looks right, but what about decimalLongitude?

```
plot(geodata_f$decimalLongitude)
hist(geodata_f$decimalLongitude)
```

It definitely looks that there is one point that is an outlayer. we can remove this point by typing:
```
geodata_f2 <- geodata_f %>% filter(decimalLongitude < -110)
```

Let's check our fourth filtered dataset.

```
plot(geodata_f2$decimalLongitude)
hist(geodata_f2$decimalLongitude)
```

Finally that things look right we can map both latitude and longitud in a single graph.
```
plot(geodata_f2$decimalLongitude, geodata_f2$decimalLatitude)
```










