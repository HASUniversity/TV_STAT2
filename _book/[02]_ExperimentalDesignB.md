#Les 2: Experimental design, vervolg

\BeginKnitrBlock{ABD}<div class="ABD">
Lees Chapter 14 (*Designing experiments*):

* 14.5 *Experiments with more than one factor*
* 14.6 *Choosing a sample size*
</div>\EndKnitrBlock{ABD}

##Power berekenen in R
De power van een toets is de waarschijnlijkheid dat, als er een verschil is, deze ook wordt aangetoond (dus een p-waarde onder de drempelwaarde \alpha).
Wanneer je een inschatting hebt van de variantie en het minimale verschil wat je wilt kunnen aantonen, dan kan je voor een gekozen aantal herhalingen berekenen wat de power is.
Andersom kan ook: je wilt een minimale power hebben (meestal minstens 0,8), dan kan je berekenen hoeveel herhalingen je daar minimaal voor nodig hebt.

In R heb je voor verschillende statistische toetsen een power-functie.
Voor de t-toets is dat `power.t.test()`.

De belangrijkste argumenten die je mee kan geven aan deze functie zijn:

* n = aantal herhalingen per groep
* delta = het absolute verschil dat je minimaal wilt aantonen
* sd = standaarddeviatie

Stel je wilt weten hoeveel herhalingen je nodig hebt om met een power van 0,8 minimaal een verschil van 1 wilt aantonen in de groei van planten tussen twee behandelingen.
De geschatte standaarddeviatie is 0.7.


```r
power.t.test(delta=1, sd=0.7, power=0.8)
```

```
## 
##      Two-sample t test power calculation 
## 
##               n = 8.764711
##           delta = 1
##              sd = 0.7
##       sig.level = 0.05
##           power = 0.8
##     alternative = two.sided
## 
## NOTE: n is number in *each* group
```

Stel, nu blijk je maar 6 herhalingen kwijt te kunnen in de proef, en je wilt weten wat dán de power wordt:


```r
power.t.test(delta=1, sd=0.7, n=6)
```

```
## 
##      Two-sample t test power calculation 
## 
##               n = 6
##           delta = 1
##              sd = 0.7
##       sig.level = 0.05
##           power = 0.6077267
##     alternative = two.sided
## 
## NOTE: n is number in *each* group
```

Heb je een gepaarde of een eensteekproef-t-toets, dan moet je dan aangeven met het argument `type="paired"` of `type="one.sample"`.



\BeginKnitrBlock{exercise}<div class="exercise"><span class="exercise" id="exr:les2"><strong>(\#exr:les2) </strong></span>
Maak de volgende *practical problems* uit het boek:

4, 9, 13, 15, 16
</div>\EndKnitrBlock{exercise}

