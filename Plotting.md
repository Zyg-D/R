**Gantt bar chart for Likert items**

```r
require(grid)          # gal ir nereikia
require(lattice)       # gal ir nereikia
require(latticeExtra)  # gal ir nereikia
require(HH)            # reikia

data(ProfChal)  ## ProfChal is a data.frame.

likert(Question ~ . , data=ProfChal[ProfChal$Subtable=="Employment sector",]
      ,main='Is your job professionally challenging?' # title
      ,col=c("red","pink","light grey","light blue","blue") # spalvos
      ,layout=c(1,1) # i kiek daliu padalinta lapo erdve, i kiek padalinta - (eilute, stulpelis)
      ,par.strip.text=list(cex=8.0) # ?
      #,box.width=0.7 # bars storumas
      ,box.ratio=2.0 # bars storumas (isreikstas per santyki su gapu), panasu, kad neveikia, kai yra bars.width
      ,ylab="ylab tekstas" # tekstas kaireje puseje vertikaliai
      ,ylab.right="" # tekstas desineje puseje vertikaliai
      ,rightAxisLabels=c("a","b","c","d","e") # is desines puses prie kiekvieno bar galima prirasyti labels. Jeigu pakeiti per kitur pakeiti orderi, cia irgi tvarkingai auto pasikeicia
      ,BrewerPaletteName="RdBu" # ?
      ,as.percent=TRUE
      ,positive.order=TRUE # surikiuoja barus nuo geriausio
      ,xlim=c(-30,100) # faktinis X asies range - susietas su grafiku (nuo,iki)
      ,scales=list(x=list(at=seq(-50,100,10) # faktiniai X asies taskai, ant kuriu bus dedami labels, tik tiek ju ir matysis (nuo, iki,step)
                         # labels seka - tai, kas matysis - siame pvz seka yra konstruojama, nes i kaire nuo 0 reikia be minuso
                         # jei reiktu %, tai naudojama, e.g., paste(seq(-60,100,10),'%',sep='')
                         ,labels=c(seq(50,0,-10),seq(10,300,10))
                         ,cex=0.8 # font size of X axis labels
                         ,relation="free" # ="same"; "free" nuima tarpa virsuje po title
                         )
                  ,y=list(cex=0.8 # font size of bar labels (Y axis)
                         ,relation="same" # ="same"; "free" nuima tarpa desineje puseje
                         )
                  )
      # legend
      ,auto.key=list(cex=0.8 # font size
                    ,between=0.3 # gap'as tarp spalvos langelio irs teksto
                    ,between.columns=2.5 # gap'as tarp kategoriju
                    )
      )
```
