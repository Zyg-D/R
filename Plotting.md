**Gantt bar chart for Likert items**

```r
require(HH)     # also loads: lattice, grid, latticeExtra

# funkcija naudojama, jeigu reikia procentiniu verciu ant grafiko, irasant panel=myPanelFunc
myPanelFunc <- function(...){
  panel.likert(...)
  vals <- list(...)
  DF <- data.frame(x=vals$x, y=vals$y, groups=vals$groups)
  
  ### some convoluted calculations here...
  grps <- as.character(DF$groups)
  for(i in 1:length(origNames)){
    grps <- sub(paste0('^',origNames[i]),i,grps)
  }
  
  DF <- DF[order(DF$y,grps),]
  
  DF$correctX <- ave(DF$x,DF$y,FUN=function(x){
    x[x < 0] <- rev(cumsum(rev(x[x < 0]))) - x[x < 0]/2
    x[x > 0] <- cumsum(x[x > 0]) - x[x > 0]/2
    return(x)
  })
  
  subs <- sub(' Positive$','',DF$groups)
  collapse <- subs[-1] == subs[-length(subs)] & DF$y[-1] == DF$y[-length(DF$y)]
  DF$abs <- abs(DF$x)
  DF$abs[c(collapse,FALSE)] <- DF$abs[c(collapse,FALSE)] + DF$abs[c(FALSE,collapse)]
  DF$correctX[c(collapse,FALSE)] <- 0
  DF <- DF[c(TRUE,!collapse),]
  
  DF$perc <- ave(DF$abs,DF$y,FUN=function(x){x/sum(x) * 100})
  ###
  
  panel.text(x=DF$correctX, y=DF$y, label=paste0(round(DF$perc,1),'%'), cex=0.7)
}

data(ProfChal)
myData = ProfChal
rm(ProfChal)
origNames = colnames(myData) # required for myPanelFunc
likert(x=Question ~ . , data=myData[myData$Subtable=="Employment sector",]
      ,aspect=1.1 # grafiko lentos aukstis dalintas is plocio
      ,main='Is your job professionally challenging?' # title
      ,ReferenceZero=3 # explicitly states the col number which is neutral. In case there is no neutral position, the reference is set like this: =2.5
      ,col=c("#E94E1B", "#F7AA4E", "#BEBEBE", "#6193CE", "#00508C") #colors
      #,border="black" # border around and inside bars
      ,horizontal=T # bar direction
      ,as.percent=TRUE
      ,positive.order=TRUE # surikiuoja barus nuo geriausio rezultato
      ,layout=c(1,1) # i kiek daliu padalinta lapo erdve, i kiek padalinta - (eilute, stulpelis). Naudinga rodant kelis grafikus, i kuriuos duomenys ateina is stulpelio "Subtable" per: x=Question ~ . | Subtable
      #,resize.height=c(2,1) # kokius santykinius dydzius atitiks kiekvienas is layout 2 parametro elementu, galima rasyti ir ="rowSums"
      #,resize.width=c(3,4) # kokius santykinius dydzius atitiks kiekvienas is layout 1 parametro elementu
      #,h.resizePanels=rowSums(your_df[,1:4])  # resize'inimas per Named num vektoriu
      #,between=list(x=0.5,y=0.5) # kai yra layout padalinimai - atstumai tarp grafiku atitinkamose asyse
      #,strip.left=strip.custom(bg="gray97") # kaireje, tarp ticks ir grafiko uzrasoma apibudinanti kategorija. Atrodo, patikimai veikia tik kai paduodama lentele (ne df) su skaitinem reiksmem abiejose asyse, e.g.: x=USAge.table[75:1, 2:1, c("1939","1959","1979")]
      #,strip=strip.custom(bg="#00508C") # virsuje, tarp ticks ir grafiko uzrasoma apibudinanti kategorija. Atrodo, patikimai veikia tik kai paduodama lentele (ne df) su skaitinem reiksmem abiejose asyse, e.g.: x=USAge.table[75:1, 2:1, c("1939","1959","1979")]
      #,par.strip.text=list(cex=0.6, lines=5) # langelio storumas ir font dydis, galiojantis ir strip, ir strip.left
      #,box.width=0.7 # bars storumas
      ,box.ratio=2.0 # bars storumas (isreikstas per santyki su gapu), panasu, kad neveikia, kai yra bars.width
      ,ylab="ylab tekstas" # tekstas kaireje puseje vertikaliai
      #,rightAxis=TRUE # jeigu nerodo, galima ijungt count rodyma desineje puseje, aktualu kai nenaudojama as.percent
      ,ylab.right="ylab.rignt tekstas" # tekstas desineje puseje vertikaliai
      #,rightAxisLabels=c("a","b","c","d","e") # is desines puses prie kiekvieno bar galima prirasyti labels. Formatas reguliuojamas: =format(rowSums(PoorChildren[,1:4]), big.mark=",") . Jeigu pakeiti per kitur pakeiti orderi, cia irgi tvarkingai auto pasikeicia
      #,BrewerPaletteName="RdBu" # cia atrodo kazkas susije su spalvom, tai pasimatytu, jeigu jos nebutu parinktos konkrecios
      ,xlab="Percentage" # tekstas po X labels
      #,xlab.top="xlab.top tekstas" #uzdeda teksta virs grafiko, virs virsutiniu labels
      ,xlim=c(-30,100) # faktinis X asies range - susietas su grafiku (nuo,iki)
      ,scales=list(x=list(at=seq(-50,100,10) # faktiniai X asies taskai, ant kuriu bus dedami labels, tik tiek ju ir matysis (nuo, iki,step)
                         # labels seka - tai, kas matysis - siame pvz seka yra konstruojama, nes i kaire nuo 0 reikia be minuso
                         # jei reiktu %, tai naudojama, e.g., paste(seq(-60,100,10),'%',sep='')
                         ,labels=c(seq(50,0,-10),seq(10,300,10))
                         ,limits=c(-30,100) # koki faktini X range talpina grafiko area. Dalis, kurios netalpina, iseina uz area ribu
                         ,cex=0.8 # font size of X axis labels
                         #,tck=c(2.1,0.9) # ilgis bruksniuku (bottom,top)
                         ,rot=c(0, 0) # pirmas parametras - kiek laipsniu pasuktas X labels tekstas
                         ,relation="free" # ="same"; "free" nuima tarpa virsuje po title
                         )
                  ,y=list(cex=0.8 # font size of bar labels (Y axis)
                         ,alternating=NULL # labels on Y axis (only if numeric?): 1=only left; 2=only right; 3=both
                         #,tck=c(2.1,0.9) # ilgis bruksniuku (left,right)
                         ,relation="free" # ="same"; "free" listina tik tuos klausimus, i kuriuos buvo atsakyta prie konkrecios kategorijos, nurodytos pvz taip: x=Question ~ . | Subtable. Subtable - kategoriju col. Taip pat nuima tarpa desineje puseje
                         )
                  )
      # legend
      ,key.border.white=T # ant spalviniu elementu permatoma borderi padaro baltu
      ,auto.key=list(cex=0.8 # font size
                    ,space="bottom" # position, e.g.: "bottom", "right"
                    #,columns=5 # i kiek stulpeliu sudalinta
                    ,reverse=F # su mano duomenim neveikia, nors su docs pvz veikia
                    ,padding.text=1.3 # elementu aukstis
                    ,between=0.3 # gap'as tarp spalvos langelio ir teksto
                    ,between.columns=2.5 # gap'as tarp kategoriju
                    )
      ,par.settings=list(layout.heights=list(bottom.padding=1.5 # didinant, atsiranda atstumas PO grafiku
                                            ,top.padding=0.1 # didinant, atsiranda atstumas VIRS grafiko
                                            ,key.axis.padding=3.5 # didinant, atsiranda atstumas tarp grafiko virsaus ir title, grafikas spaudziasi
                                            )
                        ,layout.widths=list(right.padding=1.5 # didinant, atsiranda atstumas tarp DESINIO krasto ir grafiko
                                           #,ylab.right=1.5 # didinant atsiranda atstumas DESINEJE tarp ylab.right teksto ir grafiko
                                           #,axis.key.padding=0.5 # atrodo tarsi tas pats
                                           ,key.right=1.4 # didinant, atsiranda atstumas tarp DESINEJE esancios legendos ir grafiko
                                           ,left.padding=1.5 # didinant, atsiranda atstumas tarp KAIRIO krasto ir grafiko
                                           #,ylab=2.5 # didinant atsiranda atstumas KAIREJE tarp ylab teksto ir grafiko
                                           )
                        ,axis.line=list(col="black") # the color of outside borders and ticks, e.g.: "transparent"
                        )
      ,sub="" #tekstas pacioje apacioje
      ,panel=myPanelFunc
      )

# When X labels are needed on top instead of bottom:
tmph=likert(...)
names(tmph$x.scales$labels) <- tmph$x.scales$labels
update(tmph, scales=list(x=list(alternating=2)))
```
