% Hausaufgabe 13
% Stephanie Sch�lzel <Schoelze@students.uni-marburg.de>
% 2014-05-30

Falls die Umlaute in dieser und anderen Dateien nicht korrekt dargestellt werden, sollten Sie File > Reopen with Encoding > UTF-8 sofort machen (und auf jeden Fall ohne davor zu speichern), damit die Enkodierung korrekt erkannt wird! 




# Die n�chsten Punkte sollten langsam automatisch sein...
1. Kopieren Sie diese Datei in Ihren Ordner (das k�nnen Sie innerhalb RStudio machen oder mit Explorer/Finder/usw.) und �ffnen Sie die Kopie. Ab diesem Punkt arbeiten Sie mit der Kopie. Die Kopie bitte `hausaufgabe13.Rmd` nennen und nicht `Kopie...`
2. Sie sehen jetzt im Git-Tab, dass der neue Ordner als unbekannt (mit gelbem Fragezeichen) da steht. Geben Sie Git Bescheid, dass Sie die �nderungen im Ordner verfolgen m�chten (auf Stage klicken). Die neue Datei steht automatisch da.
3. Machen Sie ein Commit mit den bisherigen �nderungen (schreiben Sie eine sinnvolle Message dazu -- sinnvoll bedeutet nicht unbedingt lang) und danach einen Push.
4. Ersetzen Sie meinen Namen oben mit Ihrem. Klicken auf Stage, um die �nderung zu merken.
5. �ndern Sie das Datum auf heute. (Seien Sie ehrlich! Ich kann das sowieso am Commit sehen.)
6. Sie sehen jetzt, dass es zwei Symbole in der Status-Spalte gibt, eins f�r den Zustand im *Staging Area* (auch als *Index* bekannt), eins f�r den Zustand im Vergleich zum Staging Area. Sie haben die Datei modifiziert, eine �nderung in das Staging Area aufgenommen, und danach weitere �nderungen gemacht. Nur �nderungen im Staging Area werden in den Commit aufgenommen.
7. Stellen Sie die letzten �nderungen auch ins Staging Area und machen Sie einen Commit (immer mit sinnvoller Message!).
8. Vergessen Sie nicht am Ende, die Lizenz ggf. zu �ndern!

Um einiges leichter zu machen, sollten Sie auch die Datei `priming.tab` aus dem Data-Ordner kopieren, stagen und commiten. Sie m�ssen ggf. Ihr Arbeitsverzeichnis setzen, wenn R die .tab-Datei nicht finden kann: Session > Set Working Directory > Source File Location

# Beschreibung des Experiments
- Aufgabe: Lexikalische Entscheidung 
- Abh�ngige Variable: Reaktionszeit, gemessen in Millisekunden (ms)
- Bedingungen (2 x 2)
    - `prime`: Englisch, Deutsch
    - `target`: Englisch, Deutsch
- Wiederholungen
    - Probanden `subj` ($n_1 = 30$)
    - Items `item` ($n_2=20$)

30 Subjects x 20 Items x 8 Bedingungen = 4800 Trials.


```r
priming <- read.table("Data/priming.tab", header = T)
```

```
## Warning: kann Datei 'Data/priming.tab' nicht �ffnen: No such file or
## directory
```

```
## Error: kann Verbindung nicht �ffnen
```

```r
priming$subj <- as.factor(priming$subj)
```

```
## Error: Objekt 'priming' nicht gefunden
```

```r
priming <- subset(priming, item <= 20)  # Filler ausschlie�en
```

```
## Error: Objekt 'priming' nicht gefunden
```

```r
priming$item <- as.factor(priming$item)
```

```
## Error: Objekt 'priming' nicht gefunden
```


## Darstellung der Daten ohne Ber�cksichtigung der Wiederholungen

```r
ggplot(data = priming) + geom_density(aes(x = RT, color = cond, fill = cond), 
    alpha = 0.3)
```

```
## Error: Objekt 'priming' nicht gefunden
```



```r
ggplot(data = priming) + geom_jitter(aes(x = cond, y = RT, color = cond, fill = cond), 
    alpha = 0.5)
```

```
## Error: Objekt 'priming' nicht gefunden
```



```r
ggplot(data = priming) + geom_violin(aes(x = cond, y = RT, color = cond, fill = cond), 
    alpha = 0.5)
```

```
## Error: Objekt 'priming' nicht gefunden
```



```r
ggplot(data = priming) + geom_boxplot(aes(x = cond, y = RT, color = cond, fill = cond), 
    alpha = 0.5)
```

```
## Error: Objekt 'priming' nicht gefunden
```


## Darstellung der Daten mit Ber�cksichtigung der Wiederholungen
Wenn wir naiv die Wiederholungen betrachten, k�nnen wir uns entweder auf den zuf�lligen Faktor **Subject** oder den zuf�lligen Faktor **Item** dadurch konzentrien, dass wir die Mittelwerte berechnen. 

### By Subject
Bei "Subject" berechnen wir die Mittelwerte aller Trials innerhalb einer Versuchsperson, d.h. wir nehmen den versuchpersonenweisen Mittelwert �ber Items hinweg. Daf�r k�nnen wir `aggregate()` nutzen.

```r
priming.by.subject <- aggregate(RT ~ cond * subj, data = priming, FUN = mean)
```

```
## Error: Objekt 'priming' nicht gefunden
```


Wir k�nnen die Tabelle, die dadurch entsteht, mit `xtable()` sch�n drucken. Bemerken Sie dabei, dass wir eine weitere Option (`results='asis'`) beim Code-Block setzen m�ssen!

```r
print(xtable(priming.by.subject), type = "html", include.rownames = FALSE)
```

```
## Error: Objekt 'priming.by.subject' nicht gefunden
```


Diese Tabelle ist etwas "lang" (ist ja in long format!). Wie k�nnten sie auch breiter machen mit der Funktion `dcast()` (`cast` f�r Data Frames) aus dem Paket `reshape2`. 

Mit Versuchspersonen als Zeilen und Bedingungen als Spalten:

```r
priming.by.subject.wide1 <- dcast(priming.by.subject, subj ~ cond, value.var = "RT")
```

```
## Error: Objekt 'priming.by.subject' nicht gefunden
```

```r
print(xtable(priming.by.subject.wide1), type = "html", include.rownames = FALSE)
```

```
## Error: Objekt 'priming.by.subject.wide1' nicht gefunden
```

 
Mit Bedingungen als Zeilen und Versuchspersonen als Spalten:

```r
priming.by.subject.wide2 <- dcast(priming.by.subject, cond ~ subj, value.var = "RT")
```

```
## Error: Objekt 'priming.by.subject' nicht gefunden
```

```r
print(xtable(priming.by.subject.wide2), type = "html", include.rownames = FALSE)
```

```
## Error: Objekt 'priming.by.subject.wide2' nicht gefunden
```


Welches Format macht am meisten Sinn?

Nat�rlich m�ssen wir auch die Daten grafisch darstellen:

```r
ggplot(data = priming.by.subject) + geom_density(aes(x = RT, color = cond, fill = cond), 
    alpha = 0.3)
```

```
## Error: Objekt 'priming.by.subject' nicht gefunden
```

```r
ggplot(data = priming.by.subject) + geom_jitter(aes(x = cond, y = RT, color = cond, 
    fill = cond), alpha = 0.5)
```

```
## Error: Objekt 'priming.by.subject' nicht gefunden
```

```r
ggplot(data = priming.by.subject) + geom_violin(aes(x = cond, y = RT, color = cond, 
    fill = cond), alpha = 0.5)
```

```
## Error: Objekt 'priming.by.subject' nicht gefunden
```

```r
ggplot(data = priming.by.subject) + geom_boxplot(aes(x = cond, y = RT, color = cond, 
    fill = cond), alpha = 0.5)
```

```
## Error: Objekt 'priming.by.subject' nicht gefunden
```


Sind alle Plots gleich gut?

### By Item
Bei "Item" berechnen wir die Mittelwerte aller Trials innerhalb eines Items, d.h. wir nehmen den itemweise Mittelwert �ber Versuchpersonen hinweg. Daf�r k�nnen wir `aggregate()` nutzen.


```r
priming.by.item <- aggregate(RT ~ cond * item, data = priming, FUN = mean)
```

```
## Error: Objekt 'priming' nicht gefunden
```


Wir wollen auch hier die Daten tabellerisch darstellen. Erstellen Sie eine Tabelle in Wide-Format f�r die Mittelwerte by Item.


```r
priming.by.item.wide1 <- dcast(priming.by.item, item ~ cond, value.var = "RT")
```

```
## Error: Objekt 'priming.by.item' nicht gefunden
```

```r
print(xtable(priming.by.item.wide1), type = "html", include.rownames = FALSE)
```

```
## Error: Objekt 'priming.by.item.wide1' nicht gefunden
```



Und *eine* passende Grafik f�r die Daten by Item sollten wir auch generieren:


```r
ggplot(data = priming.by.subject) + geom_violin(aes(x = cond, y = RT, color = cond, 
    fill = cond), alpha = 0.5)
```

```
## Error: Objekt 'priming.by.subject' nicht gefunden
```



Sehen die Daten by Subject und by Item gleich aus? Wie sehen sie im Vergleich zu den Single-Trial-Daten aus?

# Subject- und Item-Analysen

## Subject-Analyse

```r
ggplot(data = priming) + geom_density(aes(x = RT, color = cond, fill = cond), 
    alpha = 0.1) + facet_wrap(~subj)
```

```
## Error: Objekt 'priming' nicht gefunden
```



```r
priming.f1 <- ezANOVA(priming, dv = .(RT), wid = .(subj), within = .(prime, 
    target), detailed = TRUE)
```

```
## Error: Objekt 'priming' nicht gefunden
```


### ANOVA

```r
print(xtable(priming.f1$ANOVA), type = "html", include.rownames = FALSE)
```

```
## Error: Objekt 'priming.f1' nicht gefunden
```


Leider sehen die Zahlen weniger als optimal aus -- es gibt Kommastellen bei Ganzzahlen und keine nicht-Null-Stellen bei manchen kleinen Kommazahlen. Daf�r gibt es auch Optionen f�r `xtable`: `digits` (Stellen) und `display` (Darstellungsart)


```r
# xtable braucht immer eine 'extra' Angabe bei digits und display f�r die
# Row-Namen, auch wenn Sie nicht gedruckt werden s: string, d: digit, f:
# float (Gleitkommazahl), e: exponent, g: exponent, nur wenn n�tig, fg:
# float mit nicht-Null-Stellen bei floats: digits = Anzahl Stellen *nach*
# dem Komma
print(xtable(priming.f1$ANOVA, display = c("s", "s", "d", "d", "f", "f", "f", 
    "fg", "s", "g"), digits = c(0, 0, 0, 0, 2, 2, 2, 2, 0, 2)), type = "html", 
    include.rownames = FALSE)
```

```
## Error: Objekt 'priming.f1' nicht gefunden
```


Sie fragen sich evtl, woher der weitere Faktor `(Intercept)` kommt. Intercept ist der *Abschnitt* und beschreibt, wie weit weg die Basis des Models von Null ist -- hier ist sie ziemlich weit weg von Null, was Sinn macht, weil niemand eine Reaktionszeit von Null hat! 

### Spherizit�t
Die jeweiligen Faktoren haben alle zwei Stufen, also weniger als drei -- Spherizit�t ist kein Problem!

## Item-Analyse

```r
ggplot(data = priming) + geom_density(aes(x = RT, color = cond, fill = cond), 
    alpha = 0.1) + facet_wrap(~item)
```

```
## Error: Objekt 'priming' nicht gefunden
```

F�hren Sie die entsprechende Item-Analyse aus.











