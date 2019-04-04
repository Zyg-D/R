**Gantt bar chart for Likert items**

```r
require(grid)          # gal ir nereikia
require(lattice)       # gal ir nereikia
require(latticeExtra)  # gal ir nereikia
require(HH)            # reikia

data(ProfChal)  ## ProfChal is a data.frame.

likert(Question ~ . , ProfChal[ProfChal$Subtable=="Employment sector",]
       ,main='Is your job professionally challenging?' # title
       ,ylab=NULL # tekstas kaireje puseje vertikaliai
       ,as.percent=TRUE
       ,scales = list(y = list(cex = 0.9)) # font size of bar labels
       ,auto.key=list(cex=0.8) # font size in legend
       )
```
