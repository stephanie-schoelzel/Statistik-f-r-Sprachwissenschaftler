% Hausaufgabe 16
% Stephanie Schölzel <Schoelze@students.uni-marburg.de>
% 2014-06-12

Falls die Umlaute in dieser und anderen Dateien nicht korrekt dargestellt werden, sollten Sie File > Reopen with Encoding > UTF-8 sofort machen (und auf jeden Fall ohne davor zu speichern), damit die Enkodierung korrekt erkannt wird! 




# Die nächsten Punkte sollten beinahe automatisch sein...
1. Kopieren Sie diese Datei in Ihren Ordner (das können Sie innerhalb RStudio machen oder mit Explorer/Finder/usw.) und öffnen Sie die Kopie. Ab diesem Punkt arbeiten Sie mit der Kopie. Die Kopie bitte `hausaufgabe16.Rmd` nennen und nicht `Kopie...`
2. Sie sehen jetzt im Git-Tab, dass die neue Datei als unbekannt (mit gelbem Fragezeichen) da steht. Geben Sie Git Bescheid, dass Sie die Änderungen in der Datei verfolgen möchten (auf Stage klicken).
3. Machen Sie ein Commit mit den bisherigen Änderungen (schreiben Sie eine sinnvolle Message dazu -- sinnvoll bedeutet nicht unbedingt lang) und danach einen Push.
4. Ersetzen Sie meinen Namen oben mit Ihrem. Klicken auf Stage, um die Änderung zu merken.
5. Ändern Sie das Datum auf heute. (Seien Sie ehrlich! Ich kann das sowieso am Commit sehen.)
6. Sie sehen jetzt, dass es zwei Symbole in der Status-Spalte gibt, eins für den Zustand im *Staging Area* (auch als *Index* bekannt), eins für den Zustand im Vergleich zum Staging Area. Sie haben die Datei modifiziert, eine Änderung in das Staging Area aufgenommen, und danach weitere Änderungen gemacht. Nur Änderungen im Staging Area werden in den Commit aufgenommen.
7. Stellen Sie die letzten Änderungen auch ins Staging Area und machen Sie einen Commit (immer mit sinnvoller Message!).
8. Vergessen Sie nicht am Ende, die Lizenz ggf. zu ändern!

# Diamonds are forever 
Bisher haben Sie von mir mehr oder weniger vollständige Analysen bekommen, bei denen Sie im Prinzip nur einzelne Schritte einfügen müssten. Es wird allerdings langsam Zeit, dass Sie eine eigenständige Analyse ausführen. Sie haben das bei der Analyse vom Priming Experiment mittels ANOVA fast gemacht, aber auch da haben Sie viel von mir vorgefertigt bekommen. Für die Aufgaben heute werden Sie den Datensatz `diamonds` aus `ggplot2` bearbeiten. Schauen Sie sich die Beschreibung des Datensatzes an


```r
`?`(diamonds)
```

<div style="border: 2px solid black; padding: 5px; font-size: 80%;">
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN">
<html><head><title>R: Prices of 50,000 round cut diamonds</title>
<meta http-equiv="Content-Type" content="text/html; charset=utf-8">
<link rel="stylesheet" type="text/css" href="">
</head><body>

<table width="100%" summary="page for diamonds"><tr><td>diamonds</td><td align="right">R Documentation</td></tr></table>

<h2>Prices of 50,000 round cut diamonds</h2>

<h3>Description</h3>

<p>A dataset containing the prices and other attributes of
almost 54,000 diamonds. The variables are as follows:
</p>


<h3>Format</h3>

<p>A data frame with 53940 rows and 10 variables</p>


<h3>Details</h3>

 <ul>
<li><p> price. price in US dollars
(\$326&ndash;\$18,823) </p>
</li>
<li><p> carat. weight of the diamond
(0.2&ndash;5.01) </p>
</li>
<li><p> cut. quality of the cut (Fair, Good,
Very Good, Premium, Ideal) </p>
</li>
<li><p> colour. diamond colour,
from J (worst) to D (best) </p>
</li>
<li><p> clarity. a measurement
of how clear the diamond is (I1 (worst), SI1, SI2, VS1,
VS2, VVS1, VVS2, IF (best)) </p>
</li>
<li><p> x. length in mm
(0&ndash;10.74) </p>
</li>
<li><p> y. width in mm (0&ndash;58.9) </p>
</li>
<li><p> z. depth
in mm (0&ndash;31.8) </p>
</li>
<li><p> depth. total depth percentage = z /
mean(x, y) = 2 * z / (x + y) (43&ndash;79) </p>
</li>
<li><p> table. width
of top of diamond relative to widest point (43&ndash;95) </p>
</li></ul>



</body></html>

</div>

Die Aufgabe ist: eine Ausgangsfrage und die darauf folgenden Anschlussfragen statistisch zu beantworten. Sie können auch einige kleinere Fragen als Gruppe behandeln. Sie haben frei Wahl von Methoden und Fragen, aber sie müssen natürlich zueinander passen!

Mögliche Ausgangsfragen sind unter anderem:

* Was bestimmt den Preis eines Diamenten?
* Was bestimmt das Gewicht eines Diamenten? Hat Farbe oder Klarheit eine Auswirkung daruf oder bloß Volumen?
* Gibt es einen Zusammenhang zwischen den verschieden Dimensionen ("Längen")? 
* Gibt es einen Zusammenhang zwischen Farbe und Klarheit? Zwischen Farbe und Carat? Zwischen Farbe und Tiefe?
* ...

*Vergessen Sie dabei nicht, dass wir bisher nur Methoden gelernt haben, wo die abhängige Variable zumindest intervallskaliert ist!*

Sie können sich auch [das *ggplot* Buch](http://dx.doi.org/10.1007/978-0-387-98141-3) zur Inspiration anschauen, v.a. Abbildungen 4.7, 4.8, 4.9, 5.2, 5.3, 5.4, 5.6, 5.14, 7.16, 9.1  und Kapitel 2.2-2.5 könnten inspirierend wirken. Den Code zur Erstellung der Figuren findet man immer im Haupttext.

**Originale Fragestellungen und Auswertungen werden mit Bonuspunkten belohnt!** 

Hier ein paar Grafiken (auch im Buch zu finden):

```r
ggplot(diamonds, aes(x = carat, y = price)) + geom_point(alpha = 0.3) + geom_smooth(method = "lm")
```

![plot of chunk unnamed-chunk-4](figure/unnamed-chunk-41.png) 

```r
ggplot(diamonds, aes(x = carat, y = price, color = color)) + geom_point(alpha = 0.3)
```

![plot of chunk unnamed-chunk-4](figure/unnamed-chunk-42.png) 

```r
ggplot(diamonds, aes(x = carat, y = price, color = color)) + geom_point() + 
    facet_wrap(~color)
```

![plot of chunk unnamed-chunk-4](figure/unnamed-chunk-43.png) 


# Noch eine Überlegung
Haben Sie dabei explorativ oder konfirmativ gearbeitet? Was hat das für eine Auswirkung auf die Interpretation der Ergebnisse?

# Lizenz
Dieses Werk ist lizenziert unter einer CC-BY-NC-SA Lizenz.

# Analyse
UV = price
 
Haben Klarheit und/oder Schliff Einfluss auf den Preis? Wenn ja, gibt es Interaktionen?


```r
ggplot(diamonds) + geom_violin(aes(x = clarity, y = price, color = clarity, 
    fill = clarity), alpha = 0.5)
```

![plot of chunk unnamed-chunk-5](figure/unnamed-chunk-51.png) 

```r

ggplot(data = diamonds) + geom_density(aes(x = price, color = clarity, fill = clarity), 
    alpha = 0.3)
```

![plot of chunk unnamed-chunk-5](figure/unnamed-chunk-52.png) 

```r

ggplot(diamonds) + geom_violin(aes(x = cut, y = price, color = cut, fill = cut), 
    alpha = 0.5)
```

![plot of chunk unnamed-chunk-5](figure/unnamed-chunk-53.png) 

```r

ggplot(data = diamonds) + geom_density(aes(x = price, color = cut, fill = cut), 
    alpha = 0.3)
```

![plot of chunk unnamed-chunk-5](figure/unnamed-chunk-54.png) 

Lineare Regression

```r
clarity.numeric <- as.numeric(diamonds$clarity)
lm.clarity <- lm(price ~ clarity.numeric, data = diamonds)

summary(lm.clarity)
```

```
## 
## Call:
## lm(formula = price ~ clarity.numeric, data = diamonds)
## 
## Residuals:
##    Min     1Q Median     3Q    Max 
##  -4673  -2673  -1466   1151  16277 
## 
## Coefficients:
##                 Estimate Std. Error t value Pr(>|t|)    
## (Intercept)       5373.2       45.1   119.1   <2e-16 ***
## clarity.numeric   -355.6       10.3   -34.5   <2e-16 ***
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## Residual standard error: 3950 on 53938 degrees of freedom
## Multiple R-squared:  0.0216,	Adjusted R-squared:  0.0215 
## F-statistic: 1.19e+03 on 1 and 53938 DF,  p-value: <2e-16
```

```r

cut.numeric <- as.numeric(diamonds$cut)

lm.cut <- lm(price ~ cut.numeric, data = diamonds)

summary(lm.cut)
```

```
## 
## Call:
## lm(formula = price ~ cut.numeric, data = diamonds)
## 
## Residuals:
##    Min     1Q Median     3Q    Max 
##  -4151  -2894  -1537   1390  15083 
## 
## Coefficients:
##             Estimate Std. Error t value Pr(>|t|)    
## (Intercept)   4678.9       62.4    75.0   <2e-16 ***
## cut.numeric   -191.1       15.4   -12.4   <2e-16 ***
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## Residual standard error: 3980 on 53938 degrees of freedom
## Multiple R-squared:  0.00286,	Adjusted R-squared:  0.00284 
## F-statistic:  155 on 1 and 53938 DF,  p-value: <2e-16
```

Beide unabhängige Variablen haben einen signifikanten p-Wert und damit Einfluss auf den Preis.

ANOVA:

```r
ANOVA0 <- aov(price ~ clarity.numeric, data = diamonds)

ANOVA1 <- aov(price ~ cut.numeric, data = diamonds)

summary(ANOVA0)
```

```
##                    Df   Sum Sq  Mean Sq F value Pr(>F)    
## clarity.numeric     1 1.85e+10 1.85e+10    1188 <2e-16 ***
## Residuals       53938 8.40e+11 1.56e+07                   
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
```

```r

summary(ANOVA1)
```

```
##                Df   Sum Sq  Mean Sq F value Pr(>F)    
## cut.numeric     1 2.46e+09 2.46e+09     155 <2e-16 ***
## Residuals   53938 8.56e+11 1.59e+07                   
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
```

Sowohl Klarheit als auch Schliff haben eine hochsignifikante Auswirkung auf den Preis.Der F-Wert der Klarheit ist allerdings deutlich h?her

Gibt es eine Interaktion zwischen Klarheit und Schliff?


```r
ANOVA2 <- aov(price ~ cut.numeric * clarity.numeric, data = diamonds)

summary(ANOVA2)
```

```
##                                Df   Sum Sq  Mean Sq F value  Pr(>F)    
## cut.numeric                     1 2.46e+09 2.46e+09   157.9 < 2e-16 ***
## clarity.numeric                 1 1.66e+10 1.66e+10  1069.3 < 2e-16 ***
## cut.numeric:clarity.numeric     1 4.32e+08 4.32e+08    27.8 1.4e-07 ***
## Residuals                   53936 8.39e+11 1.56e+07                    
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
```


Die Ergebnisse sind hochsignifikant. Allerings haben sich die Residuals nicht ver?ndert, es konnte dadurch also nicht mehr Varianz erkl?rt werden.







