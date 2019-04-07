**Gantt bar chart for Likert items**

```r
require(grid)          # gal ir nereikia
require(lattice)       # gal ir nereikia
require(latticeExtra)  # gal ir nereikia
require(HH)            # reikia

data(ProfChal)  ## ProfChal is a data.frame.

likert(x=Question ~ . , data=ProfChal[ProfChal$Subtable=="Employment sector",]
      ,aspect=1.1 # grafiko lentos aukstis dalintas is plocio
      ,main='Is your job professionally challenging?' # title
      ,col=c("#E94E1B", "#F7AA4E", "#BEBEBE", "#6193CE", "#00508C") #colors
      ,horizontal=T # bar direction
      ,as.percent=TRUE
      ,positive.order=TRUE # surikiuoja barus nuo geriausio
      ,layout=c(1,1) # i kiek daliu padalinta lapo erdve, i kiek padalinta - (eilute, stulpelis). Naudinga rodant kelis grafikus, i kuriuos duomenys ateina is stulpelio "Subtable" per: x=Question ~ . | Subtable
      #,resize.height=c(2,1) # kokius santykinius dydzius atitiks kiekvienas is layout 2 parametro elementu
      #,resize.width=c(3,4) # kokius santykinius dydzius atitiks kiekvienas is layout 1 parametro elementu
      ,strip=T # ?
      ,par.strip.text=list(cex=8.0) # ?
      #,box.width=0.7 # bars storumas
      ,box.ratio=2.0 # bars storumas (isreikstas per santyki su gapu), panasu, kad neveikia, kai yra bars.width
      ,ylab="ylab tekstas" # tekstas kaireje puseje vertikaliai
      ,ylab.right="ylab.rignt tekstas" # tekstas desineje puseje vertikaliai
      #,rightAxisLabels=c("a","b","c","d","e") # is desines puses prie kiekvieno bar galima prirasyti labels. Default rodo countus. Jeigu pakeiti per kitur pakeiti orderi, cia irgi tvarkingai auto pasikeicia
      ,BrewerPaletteName="RdBu" # ?
      ,xlab="Percentage" # tekstas po X labels
      #,xlab.top="xlab.top tekstas" #uzdeda teksta virs grafiko, virs virsutiniu labels
      ,xlim=c(-30,100) # faktinis X asies range - susietas su grafiku (nuo,iki)
      ,scales=list(x=list(at=seq(-50,100,10) # faktiniai X asies taskai, ant kuriu bus dedami labels, tik tiek ju ir matysis (nuo, iki,step)
                         # labels seka - tai, kas matysis - siame pvz seka yra konstruojama, nes i kaire nuo 0 reikia be minuso
                         # jei reiktu %, tai naudojama, e.g., paste(seq(-60,100,10),'%',sep='')
                         ,labels=c(seq(50,0,-10),seq(10,300,10))
                         ,cex=0.8 # font size of X axis labels
                         ,relation="free" # ="same"; "free" nuima tarpa virsuje po title
                         )
                  ,y=list(cex=0.8 # font size of bar labels (Y axis)
                         ,relation="free" # ="same"; "free" nuima tarpa desineje puseje
                         )
                  )
      # legend
      ,key.border.white=T # ant spalviniu elementu permatoma borderi padaro baltu
      ,auto.key=list(cex=0.8 # font size
                    ,space="bottom" # position, e.g.: "bottom", "right"
                    #,columns=5 # i kiek stulpeliu sudalinta
                    ,reverse=F # su mano duomenim neveikia, nors su docs pvz veikia
                    ,padding.text=1.3 # elementu aukstis
                    ,between=0.3 # gap'as tarp spalvos langelio irs teksto
                    ,between.columns=2.5 # gap'as tarp kategoriju
                    )
      ,sub="" #tekstas pacioje apacioje
      )
```
