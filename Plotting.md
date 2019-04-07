**Gantt bar chart for Likert items**

```r
require(grid)          # gal ir nereikia
require(lattice)       # gal ir nereikia
require(latticeExtra)  # gal ir nereikia
require(HH)            # reikia

data(ProfChal)  ## ProfChal is a data.frame.

likert(Question ~ . , data=ProfChal[ProfChal$Subtable=="Employment sector",]
      ,main='Is your job professionally challenging?' # title
      ,ylab=NULL # tekstas kaireje puseje vertikaliai
      #,rightAxisLabels=NULL # ?
      ,as.percent=TRUE
      #,auto.key=list(cex=0.8) # font size in legend
      #,positive.order=TRUE # surikiuoja barus nuo geriausio
      ,xlim=c(-30,100) # faktinis X asies range - susietas su grafiku (nuo,iki)
      ,scales=list(x=list(at=seq(-50,100,10) # faktiniai X asies taskai, ant kuriu bus dedami labels, tik tiek ju ir matysis (nuo, iki,step)
                         # labels seka - tai, kas matysis - siame pvz seka yra konstruojama, nes i kaire nuo 0 reikia be minuso
                         # jei reiktu %, tai naudojama, e.g., paste(seq(-60,100,10),'%',sep='')
                         ,labels=c(seq(50,0,-10),seq(10,300,10))
                         ,cex=0.8 # font size of X axis labels
                         )
                  ,y=list(cex=0.8) # font size of bar labels (Y axis)
                  )
      )
```
