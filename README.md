# R

**Returning a sorted data frame on specified column**
```r
Dataset[order(-Dataset$Age),]  #desc
Dataset[order(Dataset$Age),]   #asc
```
**creating a matrix from another, taking only numeric cols**
```r
numDataset = Dataset[,sapply(Dataset, is.numeric)]
```
**extracting values from named num/int type of array**
```r
Variable_name["col_name"]
Variable_name[["col_name"]]
```
**remove cols by col number (find them by `colnames(data1)`)**
```r
data2 = data1[-(2:4)]
```
**remove rows containing NAs**
```r
data = data[!is.na(data$Embarked),]
```
**Referencing data frame cols by number**
`dataset$colName` is equivalent to `dataset[11][,]`  
**t-test**
```r
t.test(dataset$continuousVarb~dataset$categoricVarb, 
       data = bussub, 
       var.equal = TRUE, 
       alternative = "two.sided")
```
**ANOVA**
```r
anova(lm(dataset[5][,]~dataset[11][,], data = bussub))
```
**Graphically shows differences between R objects**
```r
library(diffobj)
diffPrint(obj1, obj2)
```
**creating a data frame obj**
```r
fakeddf = data.frame(col1 = c("c1", "c2"),
                     col2 = c(1, 2),
                     col3 = c("c21", "c22"),
                     col4 = c("c31", "c32"),
                     col5 = c(2, 3))
```
**Installing packages from github**
```r
install.packages("devtools")  # if not already installed
library(devtools)
install_git("https://github.com/ccolonescu/PoEdata")
```
**Nice correlogram**
```r
library(psych)
my_cor3 = corr.test(the_file, method = 'pearson')
corrplot(my_cor3$r, 
         method = "color", 
         col = colorRampPalette(c("#BB4444", "#EE9988", "#FFFFFF", "#77AADD", "#4477AA"))(200), 
         type = "upper", 
         order = "hclust", 
         addCoef.col = "black", # Add coefficient of correlation
         tl.col = "black", #Text label color
         tl.srt = 45, #Text label rotation
         # Combine with significance
         p.mat = my_cor3$r, 
         sig.level = 0.05, 
         insig = "blank", 
         diag = FALSE) # hide correlation coefficient on the principal diagonal
```
this one looks nice too:
```r
#devtools::install_github("kassambara/ggcorrplot")
library(ggplot2)
library(ggcorrplot)

# Correlation matrix
data(mtcars)
corr <- round(cor(mtcars), 1)

# Plot
ggcorrplot(corr,
           hc.order = TRUE, 
           type = "lower", 
           lab = TRUE, 
           lab_size = 3, 
           method = "circle", 
           colors = c("tomato2", "white", "springgreen3"), 
           title = "Correlogram of mtcars", 
           ggtheme = theme_bw)
```
**Correlation matrix alternatives**
```r
my_cor1 = cor(the_file, method = "pearson")

library(Hmisc)
my_cor2 = rcorr(the_file$col_1, my_file$col_2, type = 'pearson')

library(psych)
my_cor3 = corr.test(the_file, method = 'pearson')
```
