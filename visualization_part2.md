## Exploring Data Part-II


This section will address the following titles:

- Using summary statistics to explore data
 
- Exploring data using visualization
 

## Structure of the data


 First of all have a glance on the data collected using the rcode.
```{r}
custdata<- read.csv('custdata.csv',header=T,sep=',')
head(custdata)
```
## Summary Analysis

```{r}
summary(custdata)
```

## Visualization of Data

The use of graphics to examine data is called visualization. We try to follow William
Cleveland's principles for scientific visualization. Details of specific plots aside, the key
points of Cleveland's philosophy are these:

- A graphic should display as much information as it can, with the lowest possible
cognitive strain to the viewer.

- Strive for clarity. Make the data stand out.

-- Avoid too many superimposed elements, such as too many curves in the
same graphing space.

-- Find the right aspect ratio and scaling to properly bring out the details of the
data.
-- Avoid having the data all skewed to one side or the other of your graph.

- Visualization is an iterative process. Its purpose is to answer questions about the
data.

## Default plots

Defalt plots are easy to create. An example of a histogram is:

```{r}
hist(custdata$age,las=1,breaks = 50)
```

## Finer plots with ggplot2 

we use *ggplot2* to demonstrate the visualizations and graphics; of
course, other R visualization packages can produce similar graphics.

***Loading the ggplot2 library ***
 
```{r}
library(ggplot2)
```
 
A simple histogram using ggplot2 is

```{r}
ggplot(custdata) +
geom_histogram(aes(x=age),
binwidth=3, fill="green",col='red')
```

## Density plot using ggplot2


```{r}
library(scales)
ggplot(custdata) + geom_density(aes(x=income)) + scale_x_continuous(labels=dollar)

```

## Extracting a subset of data

 In the following code only consider a subset of data with reasonable age and income values using *subset* function.
 
```{r}
custdata2 <- subset(custdata,
(custdata$age > 0 & custdata$age < 100
& custdata$income > 0))
```

## Creating a scatter plot

```{r}
ggplot(custdata2, aes(x=age, y=income)) +
geom_point() + ylim(0, 200000)
```

## Creating a linear fit using ggplot2

```{r}
ggplot(custdata2, aes(x=age, y=income)) + geom_point() +
stat_smooth(method="lm") +
ylim(0, 200000)
```

## Creating a lowess function to scatter plot using ggplot2

 In R, smoothing curves are fit using the loess (or lowess) functions, which calculate
smoothed
local
linear
fits
of
the
data.
In
ggplot2,
you
can
plot
a
smoothing
curve
to
the
data
by
using
geom_smooth:

```{r}
ggplot(custdata2, aes(x=age, y=income)) +
geom_point() + geom_smooth() +
ylim(0, 200000)
```

## Hexbin plot using ggplot2


A hexbin plot is like a two-dimensional histogram. The data is divided into bins, and the
number of data points in each bin is represented by color or shading.

```{r}
library(hexbin)
ggplot(custdata2, aes(x=age, y=income)) +
geom_hex(binwidth=c(5, 10000)) +
geom_smooth(color="white", se=F) +
ylim(0,200000)
```

## Bar charts using ggplot2

The most straightforward way to visualize data is with a stacked bar
chart

***Simple bar chart***

```{r}
ggplot(custdata) + geom_bar(aes(x=marital.stat,
fill=health.ins),position='stack')
```

## Side by side Bar chart (dodge)

```{r}
ggplot(custdata) + geom_bar(aes(x=marital.stat,
fill=health.ins),
position="dodge")
```

## Filled bar chart
```{r}
bp=ggplot(custdata,aes(x=marital.stat,group=health.ins,fill=health.ins))+
  geom_bar(aes(marital.stat))
bp+geom_text(stat='count', aes(label = after_stat(count)), vjust=0.8,position = position_stack(0.8), size = 2.5)+theme(axis.text.x = element_text(angle = 90, vjust = 0.5, hjust=1))+labs(x ="Marital status", y = "Pecentage",fill="health Insurance")
```



```{r}
bp=ggplot(custdata,aes(x=marital.stat,group=health.ins,fill=health.ins))+
  geom_bar(aes(marital.stat))
bp+geom_text(stat='count', aes(label = after_stat(count)), vjust=0.8,position = position_stack(0.8), size = 2.5)+theme(axis.text.x = element_text(angle = 90, vjust = 0.5, hjust=1))+labs(x ="Marital status", y = "Pecentage",fill="health Insurance")+facet_grid(~sex)
```


```{r}
#heath insurance status
library(ggplot2)
bp=ggplot(custdata,aes(x=marital.stat,group=health.ins))+
  geom_bar(aes(fill=health.ins))
bp+geom_text(stat='count', aes(label = after_stat(scales::percent(..prop..))), vjust=0.8,position = position_stack(0.8), size = 2.5)+labs(x ="Marital status", y = "Pecentage",fill="health Insurance")+scale_y_continuous(labels = scales::percent)
```

## Facet plot using ggplot2

```{r}
ggplot(custdata2) +
geom_bar(aes(x=marital.stat), position="dodge",
fill="brown") +
facet_wrap(~housing.type, scales="free_y",ncol = 3) +theme(axis.text.x = element_text(angle = 90, hjust = 1))
```
## Descriptive analysis

```{r}
# loading external function for cross tabulation
source("http://pcwww.liv.ac.uk/~william/R/crosstab.r")
```

## Creating a new categorical variable using `cut` function

Creating am income category based on the variable income

```{r}
custdata2$incomegroup <- cut(custdata2$income, breaks = c(-Inf,19000,38350,70163,Inf), labels = c("Lower class","Middle class","Upper middle class","Upper class"))
```

```{r}
table(custdata2$incomegroup)
```

```{r}
#class-wise heath insurance status 
library(ggplot2)
bp=ggplot(custdata2,aes(x=incomegroup,group=health.ins))+
  geom_bar(aes(fill=health.ins))
bp+geom_text(stat='count', aes(label = after_stat(scales::percent(..prop..))), vjust=0.8,position = position_stack(0.8), size = 2.5)+labs(x ="Income group", y = "Pecentage",fill="health Insurance")+scale_y_continuous(labels = scales::percent)
```
