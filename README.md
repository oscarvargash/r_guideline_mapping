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

