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

## Drawing our data into digital elevation model

Plese make sure you have daca.csv and the folder "worldborders" in your working directories.
You can download "worldborders" from:

https://www.dropbox.com/s/x1krwj0pr0v65xc/worldborders.zip?dl=0

Open R and new text file. We will need to install several packages for today's excercise. Please install these packages, it will be better if you add this code to the saved script of last session.

```
install.packages("mapproj")
install.packages("raster")
install.packages("elevatr")
install.packages("rgdal")

library(dplyr)
library(mapproj)
library(raster)
library(elevatr)
library(rgdal)
```

Now we can run all the code we produced last week (in case you did not save the file)

```
## Import data and ckeck it
data <- read.csv("~/Desktop/daca/daca.csv")
head(data)

# call the column with latitude
data$decimalLatitude

# filter cells withour data
geodata <- data %>% filter(!is.na(decimalLatitude))
head(geodata)
geodata$decimalLatitude

# plot the data
plot(geodata$decimalLatitude)
hist(geodata$decimalLatitude, breaks=60)

# Cleaning of extreme latitude localities 
quantile(geodata$decimalLatitude, c(0.005, 0.995))
geodata_f <- geodata %>% filter(decimalLatitude > 37, decimalLatitude < 45.30303)
plot(geodata_f$decimalLatitude)
hist(geodata_f$decimalLatitude)
plot(geodata_f$decimalLongitude)
hist(geodata_f$decimalLongitude)

# Cleaning out extreme ocurrence identified in longitude
quantile(geodata$decimalLongitude, c(0.005, 0.995))
geodata_f2 <- geodata_f %>% filter(decimalLongitude < -118.5, decimalLongitude > -124.4014)
plot(geodata_f2$decimalLongitude)
hist(geodata_f2$decimalLongitude)
plot(geodata_f2$decimalLongitude, geodata_f2$decimalLatitude)
```

# Creating a digital elevation model baselayer
First we need to create a set of points acroos the boundig box were we plan to graph.
Looking at the data we know that a good bound box is long -125 to -119 and lat 37 to 45

Let's create a dataset with points in these bounding boxes

```
ex.df <- data.frame(x=seq(from=-125, to=-119, length.out=10), 
                    y=seq(from=37, to=45, length.out=10))

plot(ex.df$x, ex.df$y)
```
We can see that we simply have created sampling points along a cuadrant

Now we need to define or projection parameters:
```
prj_dd <- "+proj=longlat +ellps=WGS84 +datum=WGS84 +no_defs"
```

Now we can download a digital elevation tile for our map
```
elev <- get_elev_raster(ex.df, prj = prj_dd, z = 5, clip = "bbox")

plot(elev)
```

To make our map more human readable we can crop our DEM just to land areas.
In order to so, we need to import a layer with world boundaries and then create a spatial item
```
path.data <- "worldborders"
world_borders <- readOGR(dsn = path.data, layer = "TM_WORLD_BORDERS-0.3")
projection(world_borders) <- CRS(prj_dd)
```

Now using worldborders we can cut only the land portion of our DEM tile
```
elev = mask(elev, world_borders)
```

# Mapping our points into a DEM
We are ready to draw our points into a map. Let's do it step by step:

Create a blanck map of the world
```
map("world", xlim=c(-125,-119), ylim=(c(37,45)), col="gray")

```
Add mountains
```
plot(elev, col = gray.colors(25, start = 0.5, end = 1, gamma = 1, alpha = 1), add = TRUE, legend=F)
```

Create a spatial dataset with latitude, longitude, and projection. Then plot over blank
```
mysp <- mapproject(geodata_f2$decimalLongitude, geodata_f2$decimalLatitude, proj="")
points(mysp, col ="black")
```

Add states
```
map("state", xlim=c(-125,-119), ylim=(c(37,45)), col="blue", add=TRUE)
```


Congratulations !!!!!!!

