#Les 3: General Linear Models
Het idee achter de GLM is dat je de variatie in de data beschrijft met een model.
Van Biocalculus 1 en 2 weet je dat een model bestaat uit **variabelen** en **parameterwaarden**.
Ter illustratie een model voor een rechte lijn:

$y=2x+3$

In bovenstaand model zijn **x** en **y** variabelen, **2** en **3** zijn parameterwaarden.
De variabele y is de **respons**variabele en x is de **verklarende** variabele.

Stel, je verwacht een lineair verband, maar je weet niet precies hoe die loopt.
Je wilt bijvoorbeeld de hoeveelheid zetmeel meten in een oplossing (wat jullie in jaar 1 hebben gedaan).
Je weet dat er een lineair verband is tussen de concentratie en absorptie van een bepaalde golflengte.
Door het maken van een ijklijn kun je dat verband **schatten**.

\BeginKnitrBlock{exercise}<div class="exercise"><span class="exercise" id="exr:ijklijn"><strong>(\#exr:ijklijn) </strong></span>IJklijn:

* Zoek de ijklijndata op van de zetmeelproef (of kijk op Blackboard).
* Maak in R een grafiek
* Schat visueel hoe de ijklijn moet lopen, en schrijf het model ervan op.
</div>\EndKnitrBlock{exercise}

##Statistisch schatten
Het schatten van de parameterwaarden is wel te doen als je data heel netjes het model volgt.
Maar meestal heb je te maken met aardig wat extra variatie.
Doel is om de parameterwaarden zo te kiezen dat de **variantie** minimaal is.

Misschien herinner je nog de term variantie uit jaar 1.
Variantie is de gemiddelde **kwadraatafstand** tot het gemiddelde.
Minimaliseren van de variantie wordt dan **Ordinary Least Squares** genoemd (*Square* verwijst naar kwadraat), afgekort **OLS**.

\BeginKnitrBlock{exercise}<div class="exercise"><span class="exercise" id="exr:OLS"><strong>(\#exr:OLS) </strong></span>OLS

* Ga naar de volgende [website](http://rserver.has.nl/rsconnect/shiny/statistics/ols/){target="_blank"}.
* vink *Show OLS fit* aan.
* Noteer de sum of squares errors (SSE) voor een horizontale lijn door 0.
* Zoek uit bij welke waarde van **b** (terwijl **a**=0) de SSE minimaal is. Wat stelt deze waarde van b voor?
* Hoe groot is SSE als je de waardes voor a en b optimaliseert (de rode lijn valt dan samen met de zwarte regressielijn)?
</div>\EndKnitrBlock{exercise}


##F-toets
In jaar 1 zijn jullie de t-toets tegengekomen.
Hiermee test je (in het geval van een onafhankelijke t-toets) of de gemiddelden van twee groepen significant van elkaar verschillen.
Met andere woorden:

* Hoe waarschijnlijk is het dat je een minstens zo groot verschil krijgt in de **steekproef**gemiddelden, terwijl er in werkelijkheid geen verschil is (dus de kansverdeling onder de **H~0~**).
* Is die waarschijnlijkheid (de p-waarde) minder dan de drempelwaarde ($\alpha$, meestal 0,05), dan wordt de H~0~ verworpen en zeggen we dat het verschil significant is.

Wat je (of de computer) doet is een kansverdeling maken van de mogelijke verschillen in gemiddelden.

Begin twintigste eeuw heeft Ronald Fisher (zie [wikipedia](https://en.wikipedia.org/wiki/Ronald_Fisher){target="_blank"}) een alternatieve manier ontwikkeld op basis van verklaarde variantie: de **F-toets**:

$$F=\frac{MS_{verklaard}}{MS_{rest}}$$
MS staat voor de *Mean Squares*, oftewel de gemiddelde kwadraatafstand.
Deze manier van toetsen wordt variantieanalyse genoemd, of op zijn Engels *Analysis of Variance*: **ANOVA**.

Als voorbeeld de data uit de vorige oefening.
De totale variantie in de data vind je door een horizontale lijn (dus a=0) door het gemiddelde te trekken (dus b zo kiezen dat SSE minimaal is).
De gemiddelde kwadraatafstand wordt dan de gevonden SSE gedeeld door 2 (het aantal waarnemingen - 1).
Dat is MS~totaal~. 

Als je nu a en b zo kiest dat de SSE geminimaliseerd wordt, hou je restvariantie over.
Deel dit door 29 en je hebt MS~rest~.
De verklaarde variantie is MS~totaal~ - MS~rest~.

Gelukkig hoef je niet zelf te goochelen met deze varianties, daar hebben we R voor.
Wat je wel moet onthouden is dat **hoe beter de fit van het statistisch model, des te hoger F wordt**.

Met wat extra gegoochel met statistisch formules kan je aantonen dat de kansverdeling van F dezelfde is als de kansverdeling van t^2^.
De berekende waarde voor F (via bovenstaande formule) geeft precies dezelfde uitkomst als de berekende waarde voor t.
Kijk maar eens naar onderstaande figuur en de bijbehorende toetsen.


<img src="[03]_Inleiding_GLM_files/figure-html/unnamed-chunk-1-1.png" width="672" />



```r
t.test(df$lengte~df$groep, var.equal=TRUE)
```

```
## 
## 	Two Sample t-test
## 
## data:  df$lengte by df$groep
## t = -7.1998, df = 18, p-value = 1.063e-06
## alternative hypothesis: true difference in means is not equal to 0
## 95 percent confidence interval:
##  -9.529953 -5.224566
## sample estimates:
## mean in group a mean in group b 
##        12.07949        19.45674
```



```r
summary(lm(df$lengte~df$groep))
```

```
## 
## Call:
## lm(formula = df$lengte ~ df$groep)
## 
## Residuals:
##     Min      1Q  Median      3Q     Max 
## -3.3835 -1.6107  0.1688  1.2992  4.4270 
## 
## Coefficients:
##             Estimate Std. Error t value Pr(>|t|)    
## (Intercept)  12.0795     0.7245   16.67 2.17e-12 ***
## df$groepb     7.3773     1.0246    7.20 1.06e-06 ***
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## Residual standard error: 2.291 on 18 degrees of freedom
## Multiple R-squared:  0.7423,	Adjusted R-squared:  0.7279 
## F-statistic: 51.84 on 1 and 18 DF,  p-value: 1.063e-06
```

Vergelijk je de p-waardes, dan zie je dat ze precies gelijk zijn.
En de F-waarde is het kwadraat van de t-waarde (ga zelf na).

Waarom heb je dan toch de t-toets moeten leren?
Twee redenen:

* Met een t-toets kan je ook **eenzijdig** testen (H~1~: a<b)
* Bij een t-toets heb je de keuze om wel of niet de aanname te maken dat de variantie gelijk is voor beide groepen. Bij ANOVA maak je altijd die aanname!


##GLM in R
De functie voor de GLM in R is `lm(r~v)`, waarbij r de responsvariabele is en v de verklarende variabele.
Wanneer v een interval/ratio-variabele is, dan voert de lm een regressie uit.
Wanneer v een nominale variabele is dan verdeelt de lm de data in groepen (zoals in bovenstaande figuur).

De output van lm bekijk je met de functie `summary()`.
Hoe ziet dan een mogelijk script uit?


```r
library(readxl)
df <- read_excel("data.xlsx")
fit <- lm(df$r~df$v)
summary(fit)
```

De stappen uitgelegd:

* Library readxl activeren om data uit Excel te kunnen lezen.
* Data importeren uit bijv. Excel.
* GLM uitvoeren en opslaan als object met de naam fit.
* De samenvatting van de GLM bekijken.

In de volgende hoofdstukken gaan we stap voor stap de GLM uitvoeren, te beginnen met de lineaire regressie.

