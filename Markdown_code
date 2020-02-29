---
title: 'Fallstudie: Robinson Club'
author: "Piet Ginski, Yannik Sacherer, Benedikt Buschhoff, Mathis Ostern, Niklas Hess, Marius Blom"
date: "28 2 2020"
output: pdf_document
---

```{r setup, include=FALSE}
knitr::opts_chunk$set(echo = TRUE)
```


# Zielformulierung: Ist die Einführung von Bionahrung beim Robinson Club aus betriebswirtschaftlicher Sicht strategisch sinvoll?

Auf Grundlage der folgenden Fragestellungen und Hypothesen, soll berurteilt werden ob eine Einführung von Bionahrung empfehlenswert ist. Um hier eine Entscheidung treffen zu können und ein aussagekräftiges Ergebnis zu erhalten, werden wir diverse statistische Methoden und Tools verwenden.

## Fragestellungen:

(1) Welchen Einfluss hat das Essen auf das Wohlbefinden der Gäste? (Stellenwert des Essens im Vergleich zu den sonstigen Angeboten ROBINSONs)
(2) Ist ein Angebot von Bionahrung den Gästen wichtig/erwünscht/erwartet? (Wichtigkeit von Bionahrung)
(3) Was bedeutet Bio für die Gäste? (Herausstellen der Elemente von Bionahrung und der Wichtigkeit dieser Elemente)
(4) Sind die Gäste bereit für ein Angebot von Bionahrung einen Aufschlag zu zahlen? (Preissensitivität der Gäste für ein zusätzliches Angebot von Bionahrung)
(5) Wem genau ist das Angebot von Bionahrung wichtig? Wie unterscheiden sich diese Personen? (Zielgruppenidentifikation bezüglich Bionahrung)
(6) Bei welchen Lebensmittelarten ist das Angebot von Bio besonders wichtig /
unwichtig? (Wichtigkeit von Bio bei unterschiedlichen Lebensmittelarten)
(7) Welche Biosiegel sind den Gästen bekannt? (Bekanntheit Biosiegel)

## Hypothesen:

(1) Personen mit Kindern bevorzugen Bionahrung eher als Personen ohne Kinder. 
(2) Robinson Gaeste sind bereit fuer Bionahrung einen Aufschlag zu zahlen.
(3) Bionahrung besitzt das Potenzial ROBINSON als Marke zu staerken.

## Packages importieren 

```{r}
install.packages('tidyverse')
library(tidyverse)
install.packages('dplyr')
library(dplyr)
install.packages('data.table')
library(data.table)
install.packages('VIM')
library(VIM)
```

## Importieren der Daten für einen ersten Überblick

### Zunächst wird der Datensatz eingelesen. Im weiteren Verlauf werden hier die fehlenden Daten summiert und der Aufbau des Datensatzes visualisiert.
```{r}
fragebogen = read.csv('fallstudie.csv')
sum(is.na(fragebogen))
str(fragebogen)
head(fragebogen)
```

## CLEANING 

### Umgang mit fehlenden Werten

#### Drei verschiedene Arten von fehlenden Daten 

1. Missing completely at random (MCAR):
Das Fehlen der Daten (auch missings gennant) steht in keiner Beziehung zu den anderen Daten. Es ist weder von der eigentlichen Variablen noch von anderen Variablen abhängig, es ist somit komplett zufällig. 
In diesem Fall kann man die missings einfach ignorieren. 

2. Missing at random (MAR): 
Das Fehlen der Daten ist unabhängig von der Variable selbst. Allerdings gibt es eine Abhängigkeit von anderen Variablen im Datensatz. Wenn man diese Art von missings ignoriert, kann das zu einer Verzerrung der Daten führen. 

3. Missing not at random (nonignorable):
In diesem Fall fehlen die Daten aufgrund der Variable selbst, stehen also im direkten Zusammenhang mit der Varible. Dabei kann es auch sein, dass das Fehlen auch von anderen Variblen abhängt. Eine Ignorierung dieser Art von missings, kann zu einer Verzerrung des Ergebnisse führen, sowie der Beziehung zwischen den Variblen.

Wichtig für eine erfolgreiche Imputation der Daten ist es, dass zwischen den fehlenden Datenpunkten und anderen vorhandenen Datenpunkten ein Zusammenhang besteht. In unserem Fall können wir die diese Annahme ahnhand der Daten treffen.

#### Drei verschiedene Methoden mit fehlenden Daten umzugehen:

1.FEHLENDE DATEN LÖSCHEN  
Wenn der prozentuale Anteil an fehlenden Werte in einer Spalte nur wenige Prozente beträgt, dann ist die schenllste und gängiste Methode, diese mithilfe des na.omit() Befehls zu löschen. Das liegt daran, dass die Löschung von beispielsweise 1 % der Befragten Personen in der Spalte, Testergebnisse wirklich stark beeinflussen und damit die eigentliche Manipulation der Ursprungsdaten vernachlässigt werden kann. 
Wenn man diese Methode bei dem gegeben Datenset anwendet, ohne zu beachten wie hoch der prozentuale Anteil der fehlenden Werte ist, führt das dazu, dass der komplette Datensatz gelöscht wird. Somit kommt diese Methode für den überwiegenden Teil unserer Fragestellungen nicht für uns in Frage. 
  
2.FEHLENDEN DATEN MIT TEST STATISTIKEN FÜLLEN
Wenn man die fehlenden Daten mit Teststatistiken ausfüllt, wie zum Beispiel dem Arithmetischen Mittel, so macht es auch dort Sinn sich vorher anzuschauen, wie hoch der prozentuale Anteil an fehlenden Werten ist. Den auch mit dieser Methode darf der Anteil nicht zu hoch sein, da die Ergebnisse sonst zu stark verzerrt werden durch die Verringerung der Varianz und der damit verbundenen Glättung der Erhebungsdaten. Des Weiteren sollte in Betracht gezogen werden das extreme Ausreißer das arithmetische Mitel zu stark beeinflussen und damit das Ergebniss verzerren können. Für den Großteil unserer Daten spielt dieser Aspekt keine Rolle, da die Antwortmöglichkeiten häufig von einer Skala von 1-7 vorgegeben sind.
  
3.FEHLENDE DATEN MIT MACHINE LEARNING ALGORITHMEN BESTIMMEN
Eine weitere Methode mit fehlenden Daten umzugehen, ist es, diese anhand von den vorhandenen Daten zu prognostizieren. Ein Beispiel für ein einfach zu implementierenden Algorithmus wäre der KNN-Algorithmus (K-Nearest-Neighbor).
Idee hinter KNN: Die fehlenden Werte können mit den vorhandenen Werten, die dem fehlenden Wert in anderen Punkten ähnlich sind, ermittelt werden. Dabei gibt es noch die Möglichkeit den Mittelwert von den sich ähnelden Werten (die Anzahl wird durch k bestimmt) zu bilden und einzusetzten. Darüber hinaus kann man noch diesen Mittelwert mit der Distanz der einzelenen Werte multiplizieren und erhält dadurch einen gewichtete Mittelwert. 
KNN kann unter bestimmten Vorraussetzungen sehr viel genauer sein als das arithmetische Mittel, da man nicht jeden fehlenden Datenpunkt gleich behandelt. Der KNN-Algorithmus bietet die Methode jeden fehlenden Datenpunkt unter Berücksichtung von Datenpunkten die diesem Datenpunkt ähneln einzeln zu betrachten und entsprechend separat zu evaluieren.
  
Für die Fragen mit einer Normierung von 1-7 werden wir eine KNN-Imputation durchführen, da wir davon ausgehen können, dass diese Daten zufällig fehlen, da wir keine Muster in den fehlenden Angaben feststellen konnten. Darüber hinaus haben wir in unserem numerischen Datensatz (numeric_fragebogen) auch Daten mit völlig verschiedenen Normierungen, welches die Gefahr birgt, dass fehlende Daten falsch prognostiziert werden. Deshalb vereinfachen wir uns die KNN-Imputation indem wir einen Datensatz mit den Normierungen von 1-7 erstellen und diesen dann imputieren.


## Welche Spalten sind von NA's betroffen?

Durch die prozentuale Berechnung der NA's pro Spalte, verschaffen wir uns einen Überblick über die fehlenden NA's und haben direkt ein gutes Fundament, welches uns hilft zu entscheiden, wie wir mit den fehlenden Werten umgehen können.

```{r}
contains_any_na = sapply(fragebogen, function(x) any(is.na(x)))
column_names_na = names(fragebogen)[contains_any_na]
## df das anzeigt wie viel Prozent NA pro Spalte vorhanden sind
perc_na = sapply(fragebogen, function(x) sum(is.na(x))/count(fragebogen))
perc_na = as.data.frame(perc_na)
perc_na
```

## Welche Spalten (Fragen) können wir ausschließen? 

Dabei können wir aus dem Fragebogen schon erkennen, dass bestimmte Fragen Freitext als Antwort haben. Wir müssen alle Spalten exkludieren, welche auf 'txt' oder auf 'txt_val' enden. Diese Spalten müssen seperat betrachtet werden da sie keine numerischen Daten enthalten und für Kalkulationen für arithemtische Mittelwerte oder Modalwerte nicht geeignet sind. 
Des Weiteren haben die beiden Anfangsdaten "Ort" und "ID" für uns keine Relevanz. Die Text-Antworten von Frage 17 und Frage 24 sind ebenfalls vorerst für uns nicht von Interesse. Die Nominal skalierte Frage 20 muss ebenfalls ausgeschlossen werden. 

```{r}
#drop von den Text Daten
#welche Spalten haben 'txt' in ihrem Namen?
fragebogendt<-as.data.table(fragebogen)
fragebogenneu<-fragebogendt[,grep("txt",colnames(fragebogendt))]
fragebogenneu
numericfragebogen<-fragebogen[-fragebogenneu]
# einzelne nicht numerische Daten (oder im Fall von f_20 nominal skalierte Daten) werden nochmal herausgefiltert und entfernt 
numericfragebogen = numericfragebogen[-c(1:2,129,122)]
#f17_Comment ausschließen
numericfragebogen$f17_Comment = NULL
# drop von denen dessen Normierung nicht 1-7 ist 
sc_1_7_fragebogen = select(numericfragebogen, -c(f21:f25_2,f11_2:f13_3,f8:f11_1))
```

Damit haben wir die Daten die eine Normierung von 1-7 für die KNN-Imputation bereitgestellt. NA's von anderen relevanten Spalten werden in den Einzelfällen separat betrachtet und entsprechend manipuliert. Dafür dient vor allem die Variable perc_na, welche Auskunft über den prozentualen Anteil an fehlenden Werte in der entsprechenden Spalte angibt. 


## KNN Imputation
Im folgenden führen wir die KNN-Imputation für das entpsrechenden Datenset sc_1_7_fragebogen durch. Dabei wird bei der Imputation von dem KNN-Algorithmus weitere Spalten mit boolean Werten erstellt. Diese Spalten sind für unsere weiteren Berechnung unerheblich, aus diesem Grund werden sie aus dem Datenset entfernt. Eine neues Datenset ist entstanden, welches wir knn_fragebogen nennen.
Zum Schluss haben wir noch einen kleinen visuellen Vergleich angestellt, indem wir Spalten aus dem alten Datensatz mit dem knn-bereinigten Datensatz verglichen haben. Dabei ging es vor allem darum, zu schauen ob die KNN-Imputation fehlerhaft war oder bestimmte Ergebnisse völlig außerhalb des Datensatzes sind, beispielsweise neue Ausreißer mit 7 oder 1 erstellt. Dies war nicht der Fall. Es galt trotzdem zu beachten, dass Spalten mit sehr hohem Anteil an fehlenden Werte auch das Potenzial besaßen, Daten stärker zu verzerren als Spalten mit niedrigeren Anteilen. Deshalb setzten wir auch im weiteren Verlauf ein spezielles Augenmerk wenn wir Daten analysieren die einen Hohen Anteil von fehlenden Werte besitzen. (Hoch: > 10 %)

```{r}
# skalierung von 1-7 in einem df sc_1_7_fragebogen als knn_fragebogen gespeichert um fehlende Werte durch KNN zu ermitteln (Imputation)
# k=sqrt(count(fragebogen)
knn_fragebogen = kNN(sc_1_7_fragebogen,k=sqrt(count(fragebogen)))
# zusätzlichen Spalten, die durch den algo knn erstellt wurden, droppen 
knn_fragebogen = select(knn_fragebogen, -(f3_3_imp:f19_10_imp))
# double check ob auch wirklich alle NA's imputiert wurden
sum(is.na(knn_fragebogen))
# Zum visuellen Vergleich wie gut KNN Algo funktioniert hat
#mit ~ 1,3 % NA's
hist(fragebogen$f4_11, col=c("darkblue"), legend = rownames(fragebogen$f4_11), main="Wichtigkeit von Essen", xlab="Datenpunkte", ylab='Beurteilung')
hist(knn_fragebogen$f4_11, col=c("darkblue"), legend = rownames(fragebogen$f4_11), main="Wichtigkeit von Essen", xlab="Datenpunkte", ylab='Beurteilung')
#mit ~ 10 % NA's
hist(fragebogen$f4_5, col=c("green"), legend = rownames(fragebogen$f4_11), main="Wichtigkeit vom Angebot Atelier", xlab="Datenpunkte", ylab='Beurteilung')
hist(knn_fragebogen$f4_5, col=c("green"), legend = rownames(fragebogen$f4_11), main="Wichtigkeit vom Angebot Atelier", xlab="Datenpunkte", ylab='Beurteilung')
#mit ~ 25 % NA's
hist(fragebogen$f6_4, col=c("orange"), legend = rownames(fragebogen$f4_11), main="Wichtigkeit von Naturbelassenheit bei Essen", xlab="Datenpunkte", ylab='Beurteilung')
hist(knn_fragebogen$f6_4, col=c("orange"), legend = rownames(fragebogen$f4_11), main="Wichtigkeit von Naturbelassenheit bei Essen", xlab="Datenpunkte", ylab='Beurteilung')

```

## *Frage 1:* Welchen Einfluss hat das Essen auf das Wohlbefinden der Gäste? (Stellen- wert des Essens im Vergleich zu den sonstigen Angeboten ROBINSON)
Hierzu betrachten wir zunächst die 11. Antwortmöglichkeit (Essen) der Frage 4 (Wie wichtig sind ihnen die nachfolgenden Angebote in ihrem Urlaub?).  
Wir definieren also im ersten Schritt, welche Antwortmöglichkeit ausgewertet werden soll, um dann anhand eines Säulendiagramms zu verdeutlichen, wie die Befragten den Stellenwert des Essens bewerten.
```{r}
# Variablendefinition
essen_wohlbefinden = knn_fragebogen$f4_11
# Erstellung Säulendiagramm
barplot(table(essen_wohlbefinden) , main = "Wichtigkeit des Essens",xlab = "Wichtigkeit", ylab = "Anzahl der befragten Gäste", space = 0.2, cex.axis = 1, cex.names = 1, cex.lab = 1, cex.main = 1.5, col = "#69b3a2")
```
**Den Gästen ist das Essen sehr wichtig.**  

Nun gehen wir genauer auf den Stellenwert des Essens im Vergleich zu den sonstigen Angeboten ein, indem wir die gesamte Frage 4 betrachten. 
Dafür fassen wir die relevanten Spalten in einer Matrix zusammen und berechnen das arithmetisches Mittel je Antwortmöglichkeit. Diese Werte stellen wir dann vergleichend in einem Säulendiagramm dar, um dann schließlich eine finale Aussage bezüglich dem Einfluss des Essens auf das Wohlbefinden der Gäste treffen zu können.
```{r}
barplot(table(essen_wohlbefinden) , main = "Wichtigkeit des Essens",xlab = "Wichtigkeit", ylab = "Anzahl der befragten Gäste", space = 0.2, cex.axis = 1, cex.names = 1, cex.lab = 1, cex.main = 1.5, col = "#69b3a2")
# Essen ist den befragten Gästen sehr wichitg
# neue Matrix erstellen
Angebote_robinson = cbind(knn_fragebogen$f4_1,
                          knn_fragebogen$f4_2,
                          knn_fragebogen$f4_3,
                          knn_fragebogen$f4_4,
                          knn_fragebogen$f4_5,
                          knn_fragebogen$f4_6,
                          knn_fragebogen$f4_7,
                          knn_fragebogen$f4_8,
                          knn_fragebogen$f4_9,
                          knn_fragebogen$f4_10,
                          knn_fragebogen$f4_11,
                          knn_fragebogen$f4_12,
                          knn_fragebogen$f4_13,
                          knn_fragebogen$f4_14)
#colnames vergeben
numberofnamesFrage1<-c("1","2","3","4","5","6","7","8","9","10","11","12","13","14")
colnames(Angebote_robinson)<-numberofnamesFrage1
namesFrage1= c('Entertainment',
                                'Abendshows',
                                'Nite Club',
                                'Live-Musik', 
                                'Atelier', 
                                'Internetverfügbarkeit',
                                'Wellfit', 
                                'FeelGood',
                                'Sportangebote',
                                'ROBIN Store',
                                'Essen',
                                'Gesundes Essen',
                                'Ökologische produziertes Essen',
                                'Natur, Kultur')
NamenTabelleFrage1<-cbind(numberofnamesFrage1,namesFrage1)
NamenTabelleFrage1
# Arithmetisches Mittel berechnen
Angebote_robinson_mean = apply(Angebote_robinson, 2, function(x) mean(x))
# Plot
barplot(Angebote_robinson_mean , main = "Vergleich von anderen Angeboten", xlab = colnames(Angebote_robinson_mean), ylab = "Wichtigkeit", space = 0.2, cex.axis = 1, cex.names = 1, cex.lab = 1, cex.main = 1.5, col = rgb(0.8,0.1,0.1,0.6), las=1)

```
**Angebot 11 (Essen) ist den befragten Gästen am wichtigsten im Vergleich zu dem restlichen Angebot**    
**Antwort:* Das Essen hat einen sehr großen Einfluss auf das Wohlbefinden der Gäste.**      


## *Frage 2:* Ist ein Angebot von Bionahrung den Gästen wichtig/erwünscht/erwartet? (Wichtigkeit von Bionahrung)
Hier betrachten wir die 13. Antwortmöglichkeit (ökologisch produziertes Essen) der Frage 4 (Wie wichtig sind ihnen die nachfolgenden Angebote in ihrem Urlaub?).   
Das Vorgehen erfolgt analog zu Teil 1 von Frage 1. So analysieren wir mithilfe eines Säulendiagramms die Wichtigkeit von Bionahrung separat. 
```{r}
essen_bionahrung = knn_fragebogen$f4_13
barplot(table(essen_bionahrung), main = "Wichtigkeit von Bionahrung", xlab = 'Wichtigkeit', ylab = "Anzahl der Befragten", space = 0.2, cex.axis = 1, cex.names = 1, cex.lab = 1, cex.main = 1.5, col = rgb(0.8,0.1,0.1,0.6), las=1)
```
**Antwort:* Bionahrung ist den Gästen wichtig und dementsprechend auch erwünscht.**


## *Frage 3:* Was bedeutet Bio für die Gäste? (Herausstellen der Elemente von Bionahrung und der Wichtigkeit dieser Elemente) 
Wir betrachten Frage 15 (Bionahrung heißt für mich...).  
Das Vorgehen erfolgt ähnlich wie bei Frage 1 Teil 2. Wir fassen die einzelnen Spalten von Frage 15 zunächst in einer neuen Matrix zusammen, um dann die Mittelwerte zu berechnen und diese in einem Säulendiagramm zu vergleichen. Hier setzen wir zudem fest, dass alle Elemente, die im Mittel eine Wichtigkeit von mindestens 2 vorweisen, tendenziell eher von den Gästen mit Bio assoziiert werden. 
```{r}
bedeutung_bio = cbind(knn_fragebogen$f15_1,
                      knn_fragebogen$f15_2,
                      knn_fragebogen$f15_3,
                      knn_fragebogen$f15_4,
                      knn_fragebogen$f15_5,
                      knn_fragebogen$f15_6,
                      knn_fragebogen$f15_7,
                      knn_fragebogen$f15_8,
                      knn_fragebogen$f15_9,
                      knn_fragebogen$f15_10,
                      knn_fragebogen$f15_11,
                      knn_fragebogen$f15_12,
                      knn_fragebogen$f15_13,
                      knn_fragebogen$f15_14,
                      knn_fragebogen$f15_15,
                      knn_fragebogen$f15_16)
#colnames vergeben 
namesFrage2= c('Guter Geschmack',
                        'Frische und Reife',
                        'Gesundheitsbewusste Ernährung',
                        'Hochwertige Nahrung',
                        'Abwechslungsreiche Nahrung',
                        'Sportlichkeit',
                        'Individualität',
                        'Wohlfühlen',
                        'Im Trend',
                        'Naturbelassenheit',
                        'Regionale Herkunft',
                        'Genuss',
                        'artgerechte Tierhaltung',
                        'Auschluss von Gentechnik',
                        'geprüfte Qualität',
                        'Gutes Preis-/Leistungsverhältnis')
numberofnamesFrage2<-c("1","2","3","4","5","6","7","8","9","10","11","12","13","14","15","16")
colnames(bedeutung_bio)<-numberofnamesFrage2
bedeutung_bio = as.data.table(bedeutung_bio)
colnames(bedeutung_bio) <- numberofnamesFrage2
NamenTabelleFrage2<-cbind(numberofnamesFrage2,namesFrage2)
NamenTabelleFrage2
# mittelwerte der einzelnen Attribute bilden
bedeutung_bio_means = colMeans(bedeutung_bio) 
#plot
barplot(bedeutung_bio_means , main = "Was bedeutet Bio für die Gäste?", xlab = colnames(Angebote_robinson_mean), ylab = "Bedeutung", space = 0.2, cex.axis = 1, cex.names = 1, cex.lab = 1, cex.main = 1.5, col = rgb(0.8,0.1,0.1,0.6), las=1)
abline(h=2.0, col="blue", lty=3, lwd=4)
# starke Assoziation mit Mittelwert kleiner gleich 2 deshalb die Linie

```
***Antwort:* Die Elemente "Frische und Reife", "Gesundheitsbewusste Ernährung", "Hochwertige Nahrung", "Naturbelassenheit", "Artgerechte Tierhaltung", "Ausschluss von Gentechnik" und "Geprüfte Qualität" werden von den Gästen besonders mit Bio assoziiert.** 




## *Frage 4:* Sind die Gäste bereit für ein Angebot von Bionahrung einen Aufschlag zu zahlen? (Preissensitivität der Gäste für ein zusätzliches Angebot von Bionahrung)
Hier wird die Frage 17 (Welchen Aufschlag auf Ihren aktuellen Reisepreis wären Sie bereit zu zahlen, wenn zusätzlich zertifizierte Bionahrung im Angebot enthalten wäre?) betrachtet.  
Wir gehen so vor, dass wir zunächst diejenigen Personen, die bei der Frage keine Angabe gemacht haben, aussortieren. Die übrigen Personen werden dann hinischtlich ihrer Preissensitivität analysiert, indem wir schauen, wie viele Personen überhaupt zu einem Aufschlag bereit wären und wie viel Aufschlag diese mindestens zahlen würden. 
```{r}
price_sensitivity = numericfragebogen$f17 
price_sensitivity = as.data.table(price_sensitivity)
price_sensitivity = na.omit(price_sensitivity)
# Anzahl an Befragten
length(numericfragebogen$f17)
# Anzahl an Befragten mit Angabe
count(price_sensitivity)
# Wie viele würden überhaupt mehr bezahlen?
ps_0 = price_sensitivity[price_sensitivity>0, .N]
# Wie viele würden mehr als 3 % bezahlen?
ps_3 = price_sensitivity[price_sensitivity>3, .N]
u_ps_3 = ps_3 * 0.03 
# Wie viele würden mehr als 5 % bezahlen?
ps_5 = price_sensitivity[price_sensitivity>5, .N]
u_ps_5 = ps_5 * 0.05
# Wie viele würden mehr als 10 % bezahlen?
ps_10 = price_sensitivity[price_sensitivity>10, .N]
u_ps_10 = ps_10 * 0.1
# Wie viele würden mehr als 15 % bezahlen?
ps_15 = price_sensitivity[price_sensitivity>15, .N]
u_ps_15 = ps_15 * 0.15
# Wie viele würden mehr als 20 % bezahlen? 
ps_20 = price_sensitivity[price_sensitivity>20, .N]
u_ps_20 = ps_20 * 0.2
```
**Von den 219 befragten Personen haben 100 Leute eine Angabe getätigt und von diesen sind 66 bereit einen Aufschlag zu zahlen. 47 Personen sind zu einem Zuschlag von mind. 3% bereit, 26 sind zu einem Zuschlag von mind. 5% bereit, 7 zu einem Zuschlag von mind. 10%, 5 zu einem Zuschlag von mind. 15% und 2 zu einem Zuschlag von mehr als 20%.**  

Nun wollen wir dies noch einmal in einem Säulendiagramm darstellen und zusätzlich zeigen, welcher Aufschlag sich lohnen würde wenn man die prozentuale Erhöhung des Umsatzes und die Kaufbereitschaft der Kunden betrachtet.
```{r}
# Ergebnisse in einem Vektor zusammenfassen
all_ps = as.data.table(c(ps_3, ps_5, ps_10, ps_15, ps_20))
all_u_ps = as.data.table(c(u_ps_3,u_ps_5,u_ps_10,u_ps_15,u_ps_20))
all_ps_transpose = t(all_ps)
all_ps_u_transpose = t(all_u_ps)
# Spaltennamen festlegen 
colnames_all_ps_transpose = c('>3%','>5%','>10%','>15%','>20%')
colnames(all_ps_transpose) = colnames_all_ps_transpose
colnames(all_ps_u_transpose) = colnames_all_ps_transpose
# Säulendiagramm zur Visualisierung
barplot(all_ps_transpose/length(numericfragebogen$f17), main = "Anzahl der Gäste die Aufschlag zahlen würden", ylab = "Anzahl", space = 0.2, cex.axis = 1, cex.names = 1, cex.lab = 1, cex.main = 1.5, col = rgb(0.8,0.1,0.1,0.6), las=1)
# Säulendiagramm lohnenswerter Aufschlag
barplot(all_ps_u_transpose , main = "Anzahl multipliziert mit dem Aufschlag", ylab = "Anzahl", space = 0.2, cex.axis = 1, cex.names = 1, cex.lab = 1, cex.main = 1.5, col = rgb(0.8,0.1,0.1,0.6), las=1)
```
  ***Antwort:* Generell besteht eine Bereitschaft einen Aufschlag für Bionahrung zu bezahlen, jedoch können wir aus unseren Ergebnissen schließen, dass eine erhöhte Preissensitivität bei Bionahrung vorhanden ist. Dies stellt die finanzielle Tragfähigkeit von Bionahrung in Frage, da Bionahrung im Regelfall teurer ist als 3%.**    



# *Frage 5:* Wem genau ist das Angebot von Bionahrung wichtig? Wie unterscheiden sich diese Personen? (Zielgruppenidentifikation bezüglich Bionahrung)

```{r}
# Filtern nach Personen, welche Bio als wichtig/unwichtig ansehen
 Biowichtig<-fragebogendt[f4_13 <= 3]
 Biounwichtig<-fragebogendt[f4_13>3]
# die Demografischen Daten aus den filtern
 Biowichtig<-Biowichtig[,c(2,144:134)]
 Biounwichtig<-Biounwichtig[,c(2,144:134)]
# Identifizieren des Durschnittlichen Einkommens
 EkBiowichtig<-Biowichtig[,mean(f26,na.rm=TRUE)]
 EkBiounwichtig<-Biounwichtig[,mean(f26,na.rm=TRUE)]
 t.test(Biowichtig$f26,Biounwichtig$f26)
# 4. Durschnittliches Alter 
 hist(Biowichtig$f22,main="Verteilung des Alters",xlab = "Alter", ylab= "Anzahl beim zugehörigen Alter", col ='#69b3a2', las=2, breaks = 10)
 hist(Biounwichtig$f22,main="Verteilung des Alters",xlab = "Alter", ylab= "Anzahl beim zugehörigen Alter", col ='#69b3a2', las=2, breaks = 10)
# 5. In welchen der beiden Orten
 BiowichtigOrt<-Biowichtig[,.N,Ort]
 ProzentwichtigOrt1<-BiowichtigOrt[1,2]/BiowichtigOrt[,sum(N)]
 ProzentwichtigOrt2<-BiowichtigOrt[2,2]/BiowichtigOrt[,sum(N)]
 BiounwichtigOrt<-Biounwichtig[,.N,Ort]
 ProzentunwichtigOrt1<-BiounwichtigOrt[1,2]/BiounwichtigOrt[,sum(N)]
 ProzentunwichtigOrt2<-BiounwichtigOrt[2,2]/BiounwichtigOrt[,sum(N)]
 GesamtprozenteOrt<-cbind(ProzentwichtigOrt1, ProzentwichtigOrt2, ProzentunwichtigOrt1, ProzentunwichtigOrt2)
 colnames(GesamtprozenteOrt)<-c("wichtig Ort 1", "wichtig Ort 2", "unwichtig Ort 1", "unwichtig Ort 2")
 GesamtprozenteOrt
# 6.woher kommen die Kunden
 WohnortBioWichtig<-Biowichtig[,.N,f25_1]
 WohnortBioUnwichtig<-Biounwichtig[,.N,f25_1]
# 7. Bildungsabschluss
 hist(Biowichtig$f20, main="Verteilung des Bildungsabschlusses, bei Personen die Bio für wichtig halten",xlab = "Bildungsabschluss", ylab= "Anzahl der Personen mit dem Abschluss", col ='#69b3a2', las=2, breaks = 4)
 hist(Biounwichtig$f20, main="Verteilung des Bildungsabschluss, bei Personen die Bio für nicht wichtig halten",xlab = "Bildungsabschluss", ylab= "Anzahl der Personen mit dem Abschluss", col ='#69b3a2', las=2, breaks = 4)
# 8. Geschlecht
 GeschlechtBiowichtig<-Biowichtig[,.N,f21]
 Prozentmännerwichtig<-GeschlechtBiowichtig[1,2]/GeschlechtBiowichtig[,sum(N)]
 Prozentfrauenwichtig<-GeschlechtBiowichtig[2,2]/GeschlechtBiowichtig[,sum(N)]
 GeschlechtBioUnwichtig<-Biounwichtig[,.N,f21]
 ProzentmännerUnwichtig<-GeschlechtBioUnwichtig[1,2]/GeschlechtBioUnwichtig[,sum(N)]
 Prozentfrauenunwichtig<-GeschlechtBioUnwichtig[2,2]/GeschlechtBioUnwichtig[,sum(N)]
 GesamtzProzenteGeschlecht<-cbind(Prozentfrauenunwichtig,Prozentfrauenwichtig,ProzentmännerUnwichtig,Prozentmännerwichtig)
 colnames(GesamtzProzenteGeschlecht)<-c("Anteil Frauen Bio unwichtig", "Anteil Frauen Bio wichtig", "Anteil Männer Bio unwichtig", "Anteil Männer Bio wichtig")
 GesamtzProzenteGeschlecht
# 9. Wieviele interessierten sich generell für Bionahrung 
 Biowichtig[Biowichtig$f22,.N]
 Biounwichtig[Biounwichtig$f22,.N]
 
 
```
***Antwort:* Die Zielgruppe, die für die Einführung von Bio interessant wäre, verdient etwa 5.000€ oder mehr, ist kurz vor 1970 geboren, bucht vor allem im Ort 1, hat einen höheren Bildungsabschluss und besteht vermehrt aus Männer.
 Allerdings lässt sich noch hinzufügen, dass die allermeisten Gäste Bio für wichtig halten (180 halten es für wichtig und nur 30 für unwichtig), was den Vergleich der beiden unterschiedlichen Gruppen erschwert. Ebenfalls unterschieden sich auch die einzelnen Gruppen nicht besonders stark, was vor allem beim Einkommen auffiel. Es gab zwar eine Tendenz, dass die Personen die Bio für unwichtig halten mehr verdienen würden, dieser Unterschied war allerdings nicht signifikant.


## *Frage 6:* Bei welchen Lebensmittelarten ist das Angebot von Bio besonders wichtig/unwichtig? (Wichtigkeit von Bio bei unterschiedlichen Lebensmittelarten)
Hierzu haben wir die Frage 16 (Wie wichtig ist Ihnen „BIO“ bei den folgenden Lebensmittelkategorien?) betrachtet.  
Zunächst haben wir erneut die Spalten der Frage 16 in einer Matrix zusammengefasst, das arithmetische Mittel berechnet und dann in einem Säulendiagramm veranschaulicht, wie wichtig/unwichtig Bio bei unterschiedlichen Lebensmittelarten sind. 

```{r}
l_bio_wichtigkeit = cbind(knn_fragebogen$f16_1,
                          knn_fragebogen$f16_2,
                          knn_fragebogen$f16_3,
                          knn_fragebogen$f16_4,
                          knn_fragebogen$f16_5,
                          knn_fragebogen$f16_6,
                          knn_fragebogen$f16_7,
                          knn_fragebogen$f16_8,
                          knn_fragebogen$f16_9)
# Arithmetisches Mittel berechnen
l_bio_wichtigkeit_mean = apply(l_bio_wichtigkeit, 2, function(x) mean(x))
#tranpose und dt kreieren
l_bio_wichtigkeit_mean = as.data.table(l_bio_wichtigkeit_mean)
l_bio_wichtigkeit_mean = t(l_bio_wichtigkeit_mean)
#colnames
numberofnamesFrage3<-c("1","2","3","4","5","6","7","8","9")
namesFrage3<-c('Milchprodukte',
               'Gemüse',
               'Obst',
               'Fischprodukte',
               'Fleischprodukte', 
               'Backwaren',
               'Süßigkeiten',
               'Gewürze',
               'Getränke'
)
colnames(l_bio_wichtigkeit_mean) =numberofnamesFrage3
NamensTabelle3<-cbind(numberofnamesFrage3,namesFrage3)
NamensTabelle3
barplot(l_bio_wichtigkeit_mean, main = "Wichtigkeit von Bio bei unterschiedlichen Lebensmittelarten", ylab = "Wichtigkeit",xlab=colnames(l_bio_wichtigkeit_mean), space = 0.2, cex.axis = 1, cex.names = 1, cex.lab = 1, cex.main = 1.5, col = "#69b3a2", las=1)

```
***Antwort:*Bei Gemüse & Salat (2), Obst (3) und Fleischprodukten (5) ist Bio für die Gäste besonders wichtig. Bei Süßigkeiten (7), Gewürzen (8) und bei den Getränken (9) ist Bio besonders unwichtig.**



## *Frage 7:* Welche Biosiegel sind den Gästen bekannt? (Bekanntheit Biosiegel)
Es wird die Frage 12 (Welche der folgenden Biosiegel kommen Ihnen bekannt vor?) berücksichtigt.  
Da die Frage 7 nicht im knn_fragebogen-Datensatz vorhanden ist, schauen wir in den fragebogendt-Datensatz. Hier müssen wir also im ersten Schritt beurteilen, wie wir mit den fehlenden Daten verfahren. Da die Anzahl an NA´s sehr klein ist, entscheiden wir uns dafür die Zeilen, die fehlende Daten enthalten, zu eliminieren. Im Anschluss können wir dann für jedes Siegel die Summe an Nennungen bilden und dies in einem Säulendiagramm darstellen. Hier legen wir zudem fest, dass die Biosiegel ab einem Wert von 122 als "den Gästen bekannt" bezeichnet werden können.

```{r}
# Wie viele NA's pro Spalte? 
na_12_n = perc_na[perc_na$f12_1:perc_na$f12_9]
# Neue Matrix
dt_f12 = as.data.table(cbind(fragebogendt$f12_1,
                 fragebogendt$f12_2,
                 fragebogendt$f12_3,
                 fragebogendt$f12_4,
                 fragebogendt$f12_5,
                 fragebogendt$f12_6,
                 fragebogendt$f12_7,
                 fragebogendt$f12_8,
                 fragebogendt$f12_9))
# Elimination der fehlenden Werte
dt_f12 = na.omit(dt_f12)
# Bilden der Summe pro Siegel
dt_f12_count = apply(dt_f12, 2, function(x) sum(x))
# Data Table kreieren
dt_f12_count = as.data.table(dt_f12_count)
dt_f12_count_transp = t(dt_f12_count)
# Spaltennamen 
colnames_dt_f12_count = c('Ecovin', 'Biopark', 'Ecoland', 'Gäe', 'Bioland', 'Demeter', 'Naturland', 'Bio', 'Bio Kreis')
colnames(dt_f12_count_transp) = colnames_dt_f12_count
# Säulendiagramm
barplot(dt_f12_count_transp, main = "Wichtigkeit von Bio bei unterschiedlichen Lebensmittelarten", ylab = "Wichtigkeit", space = 0.2, cex.axis = 1, cex.names = 1, cex.lab = 1, cex.main = 1.5, col ='#69b3a2', las=2)
abline(h=122, col="blue", lty=3, lwd=4)
```
***Antwort:* Die Biosiegel, die den Befragten bekannt sind, sind Demeter, Bioland und Bio.**      



# Hypothesen 

## *1. Hypothese:* Personen mit Kindern bevorzugen Bionahrung eher als Personen ohne Kinder.
Zunächst werden die Nullhypothese ("Das was wir widerlegen wollen") und die Alternativhypothese ("Das was wir beweisen wollen") aufgestellt.  
H0: Personen mit Kindern bevorzugen Bionahrung nicht eher als Personen ohne Kinder.  
H1: Personen mit Kindern bevorzugen Bionahrung eher als Personen ohne Kinder.    

```{r}
#Wie kommen wir an alle Personen die Kinder haben? --> Frage 2.3 sehr passend da bei der niedrigen prozentualen Anzahl an fehlenden Werten Reihen gedropped werden können.
# Annahme: Wer mit Familie kommt, kommt auch mit Kindern
# Frage 9
preferences_bio = as.data.table(fragebogen$f9)
# Frage 2
bio_children = as.data.table(fragebogen$f2)
#replacing 0 with NA
bio_children[bio_children == 0] <- NA
#datasets verbinden 
merge_p_b = cbind(bio_children, preferences_bio)
#Spalten Namen ändern
colnames(merge_p_b) = c('bio_children','preferences_bio')
#wie viele NA's pro Spalte? 
perc_na$f9.n #0.04 % NA
perc_na$f2.n #keine NA
#es kann bei so einer geringen Anzahl an NA's Reihen gedroped werden 
merge_p_b_na = na.omit(merge_p_b)
#filtern nach denen die bio mögen
#bio_mögen_u.kinder = merge_p_b_na[merge_p_b_na$preferences_bio < 3 & merge_p_b_na$bio_children == 3]
```


Im ersten Schritt ergibt sich die Präferenzeinstellung der Befragten aus der Frage 9 im Fragebogen. Dementsprechend definieren wir zunächst eine neue Matrix mit der Präferenzeinstellung der befragten Gästen.  
Weiterhin stellt sich dann erstmal die Frage, wie wir an alle Personen kommen, die Kinder haben. Hierfür eignet sich Frage 23 ("Alter Ihrer Kinder in Jahren (wenn vorhanden)"), wenn wir unterstellen, dass diejenigen, die eine Angabe machen, Kinder haben. Somit können wir davon ausgehen, dass 4 NA´s dafür sprechen, dass die Person keine Kinder hat, da sie kein Alter angegeben hat.  
Nun können wir die Mittelwerte für die Präferenzeinstellung gegenüber Bionahrung von Personen mit und ohne Kinder berechnen und miteinander vergleichen. Im Rahmen eines beidseitigen T-Tests wird jetzt also geprüft, ob es einen signifikanten Unterschied zwischen der mittleren Präferenzeinstellung der beiden Personengruppe gibt.
```{r}
# Neue Matrix (Präferenzeinstellung)
preferences_bio = as.data.table(fragebogen$f9)
# Neue Matrix (Kinder)
kinder_bio_all = cbind(fragebogen$f23_1, fragebogen$f23_2, fragebogen$f23_3, fragebogen$f23_4, preferences_bio)
# Neue Spalte, die zählt wie viele NA´s pro Reihe vorhanden sind
kinder_bio_all$na_count <- apply(kinder_bio_all, 1, function(x) sum(is.na(x)))
kinder_bio_all =  as.data.table(kinder_bio_all) 
colnames(kinder_bio_all) = c('1Ki','2Ki','3Ki','4Ki','bio','na_count')
# Definition der Personen mit Kindern und ohne Kinder 
keine_kinder = kinder_bio_all[kinder_bio_all$na_count==4]
kinder = kinder_bio_all[kinder_bio_all$na_count!=4]
# Elimination der fehlenden Daten und Erstellung Data Table
kinder_bio = as.data.table(kinder$bio)
kinder_bio = na.omit(kinder$bio)
keine_kinder_bio = as.data.table(keine_kinder$bio)
# Mittelwert ermitteln 
mean_k_b = mean(kinder_bio) 
mean_kk_b = mean(data.matrix(keine_kinder_bio))
# T-Test durchführen
t.test(kinder_bio, keine_kinder_bio)
```
***Ergebnis:* Der p-Wert ist größer als das Signifikanzniveau Alpha (=5%) somit können wir H0 nicht verwerfen; das Ergebnis spricht für die Nullyhpothese.  Also lehnen wir die Hypothese ab, dass Personen mit Kindern Bionahrung eher bevorzugen als Personen ohne Kinder.**



## *2. Hypothese:* Robinson Gäste sind bereit für Bionahrung einen Aufschlag zu zahlen
Zunächst werden erneut wieder die Nullhypothese ("Das was wir widerlegen wollen") und die Alternativhypothese ("Das was wir beweisen wollen") aufgestellt.  
H0 (Nullhypothese) : Die Robinson Gäste sind nicht bereit für Bionahrung einen Aufschlag zu zahlen.  
H1 (Alternativhypothese) : Die Robinson Gäste sind bereit für Bionahrung einen Aufschlag zu zahlen.  

Um die Hypothese zu prüfen, wird die Frage 18 mit der Aussage 2 ("Für ein Angebot von Bionahrung im Urlaub zahle ich gerne etwas mehr") und der Aussage 9 ("Aufgrund des Angebots von Bionahrung mehr zu zahlen, würde mich eher abschrecken") analysiert.  
Nachdem zuerst eine neue Matrix mit den Daten der zweiten Aussage erstellt wird, wird mit einem Histogramm die generelle Bereitschaft bezüglich eines Aufschlags ermittelt.  
Im Anschluss wird dann erneut eine Matrix und ein Histogram mithilfe der Daten aus der neunten Aussage erstellt, um mehr Daten miteinzubeziehen und somit die Aussagekräftigkeit der Aussage über die Bereitschaft zu verstärken.  
Im letzten Schrtt kann dann noch ein einseitiger T-Test durchgeführt werden. Es wird geschaut, ob der Mittelwert der zweiten Aussage signifikant vom Wert 4 (= neutral) abweicht. Somit kann geschaut werden, ob die Befragten der Aussage eher zustimmen (wenn Mittelwert signifikant nach unten abweicht) oder diese eher ablehnen (wenn Mittelwert signifikant nach oben abweicht). Wenn der Mittelwert signifikant nach unten abweicht, würde dies also für unsere Alternativhypothese sprechen.  
Man könnte umgekehrt auch einen einseitigen T-Test der Aussage 9 machen. Wenn der Mittelwert signifikant von 4 nach oben abweicht, würde dies dann für unsere Alternativhypothese sprechen.
```{r}
# Neue Matrix (Aussage 2)
bio_aufschlag = as.matrix(knn_fragebogen$f18_2)
# Histogramm (Aussage 2)
hist(bio_aufschlag, main = "Kaufbereitschaft von Bionahrung",xlab = 'Wichtigkeit' ,ylab = "Anzahl der Gäste", col ='#69b3a2', las=2, breaks=7)
# Neue Matrix (Aussage 9)
aufschlag_abschrecken = as.matrix(knn_fragebogen$f18_9)
# Histogramm (Aussage 9)
hist(aufschlag_abschrecken, main = "Abschreckung durch Aufschlag bei Bionahrung",xlab = 'Wichtigkeit' ,ylab = "Anzahl der Gäste", col ='#69b3a2', las=2, breaks = 7)
# Einseitiger T-Test (Aussage 2)
t.test(bio_aufschlag, mu = 4, alternative = 'less')
# Zusätzlicher möglicher T-Test (Aussage 9)
t.test(aufschlag_abschrecken, mu = 4, alternative = 'greater')
```
***Ergebnis:* Da der p-Wert kleiner als das Signifikanzniveau Alpha (5%) ist, wird die Alternativhypothese angenommen. Somit nehmen wir an, dass die Robinson Gäste bereit sind für Bionahrung einen Aufschlag zu zahlen.**      

## *3. Hypothese:* Bionahrung besitzt das Potenzial ROBINSON als Marke zu stärken.

Zur Prüfung dieser Hypothese haben wir uns entschieden, die Fragen 3 und 15 heranzuziehen.  
Die Idee ist zu schauen, für was Bionahrung steht und gleichzeitig zu schauen, in welchen Punkten die Marke Robinson noch Verbesserungsbedarf aufweist. Somit können wir dann entscheiden, ob die jeweiligen Eigenschaften der Marke Bio das Potential haben, die Marke Robinson zu stärken.  
Dazu werden die vergleichbaren Paare in einem Säulendiagramm miteinander verglichen und am Ende noch je Vergleichspaar ein T-Test durchgeführt, um die Signifkanz der Verbesserungspotentiale zu bestimmen.   

### Frage 1: Was verbinden die Kunden mit der Marke Robinson?
Es wird zunächst Frage 3 ("Was verbinden Sie mit der Marke ROBINSON?") betrachtet, um zu schauen, was die Kunden derzeitig mit Robinson verbinden.  
Hierbei errechnen wir die Mittelwerte der Aussagen von Frage 3 und vergleichen diese in einem Säulendiagramm. Zudem berechnen wir auch noch die einzelnen Modalwerte und stellen auch diese in einem Säulendiagramm dar. Hierbei definieren wir, dass bei einem Wert von 2 oder kleiner eine starke Assoziation mit der Marke Robinson bzw. Bio vorliegt.  
```{r}
#für marke robinson relevanten spalten zusammenführen 
marke_robinson = cbind(knn_fragebogen$f3_1,knn_fragebogen$f3_2,knn_fragebogen$f3_3,knn_fragebogen$f3_4,knn_fragebogen$f3_5,knn_fragebogen$f3_6,knn_fragebogen$f3_7,knn_fragebogen$f3_8,knn_fragebogen$f3_9,knn_fragebogen$f3_10, knn_fragebogen$f3_11)
#mean ausrechnen 
col_means_r=colMeans(marke_robinson)
#columnnames erstellen
nameshypothese3.0 = c('Gutes Preis- / Leistungsverhältnis',
                   'Umfassendes Wohlfühlangebot',
                   'Sportlichkeit',
                   'Kinderfreundlichkeit',
                   'Hochwertiges Speisenangebot',
                   'Abwechslungsreiches Speisenangebot',
                   'Einzigartigkeit',
                   'Hohe Qualität',
                   'Gesundheit',
                   'ROBINSON ist „in“',
                   'Genuss')
numberofnamesHypothese3.0<-c("1","2","3","4","5","6","7","8","9","10","11")
NamenTabelleHypothese3.0<-cbind(nameshypothese3.0,numberofnamesHypothese3.0)
NamenTabelleHypothese3.0
#colnames vergeben
col_means_r = as.data.table(col_means_r)
col_means_r_transp = t(col_means_r)
colnames(col_means_r_transp) = numberofnamesHypothese3.0
#plot 
barplot(col_means_r_transp, main = "Was verbinden die Kunden mit der Marke Robinson", ylab = "Bedeutung", col = 'black', las=1)
abline(h=2.0, col="lightgreen", lty=3, lwd=4)
#weiteren Wert in Betracht ziehen: Modalwert 
## mode function definieren 
modefunc <- function(x){
  tabresult <- tabulate(x)
  themode <- which(tabresult == max(tabresult))
  if(sum(tabresult == max(tabresult))>1) themode <- NA
  return(themode)
}
#modalwerte erechnen
col_mode_r = apply(marke_robinson, 2, modefunc)
#colnames vergeben
col_mode_r_transp = t(col_mode_r)
colnames(col_mode_r_transp) = numberofnamesHypothese3.0
#plot
barplot(col_mode_r_transp, main = "Was verbinden die Kunden mit der Marke Robinson",ylab = "Bedeutung", col = "black", las=1)

```
***Ergebnis:* Die Attribute 'Umfassendes Wohlfühlangebot', 'Sportlichkeit', 'Kinderfreundlichkeit', 'Hochwertiges Speisenangebot', 'Abwechslungsreiches Speisenangebot', 'hohe Qualität werden von den Gästen besonders mit der Marke Robinson assoziert.**


### Frage 2: Was verbinden die Kunden mit der 'Marke' Bio?
Anhand der Frage 15 können wir nun schauen, was die Kunden mit der "Marke" Bio verbinden.   
Wir gehen hier genau so wie bei Frage 1 vor.
```{r}
#für marke bio relevanten spalten zusammenführen 
marke_bio = cbind(knn_fragebogen$f15_1,knn_fragebogen$f15_2,knn_fragebogen$f15_3,knn_fragebogen$f15_4,knn_fragebogen$f15_5,knn_fragebogen$f15_6,knn_fragebogen$f15_7,knn_fragebogen$f15_8,knn_fragebogen$f15_9,knn_fragebogen$f15_10, knn_fragebogen$f15_11,knn_fragebogen$f15_12,knn_fragebogen$f15_13,knn_fragebogen$f15_14,knn_fragebogen$f15_15,knn_fragebogen$f15_16)
#mean ausrechnen 
col_means_b=colMeans(marke_bio)
#colnames 
nameshypothese3.1= c('guter Geschmack',
                   'Frische und Reife',
                   'gesundheitsbewusste Ernährung',
                   'Hochwertige Nahrung',
                   'Abwechslungsreiche Nahrung',
                   'Sportlichkeit',
                   'Individualität',
                   'Wohlfühlen',
                   'Bio ist „in“',
                   'Naturbelassenheit',
                   'regionale Herkunft',
                   'Genuss',
                   'artgerechte Tierhaltung',
                   'Ausschluss von Gentechnik',
                   'geprüfte Qualität (Gütesiegel)',
                   'Gutes Preis- / Leistungsverhältnis')
numberofnamesHypothese3.1<-c("1","2","3","4","5","6","7","8","9","10","11","12","13","14","15","16")

marke_bio= as.data.table(marke_bio)
NamenTabelleHypothese3.1<-cbind(numberofnamesHypothese3.1,nameshypothese3.1)
NamenTabelleHypothese3.1
col_mode_r_transp = t(col_means_b)
colnames(col_mode_r_transp) = numberofnamesHypothese3.1
#plot 
barplot(col_mode_r_transp, main = "Was verbinden die Kunden mit Robinson", ylab = "Bedeutung", col = "lightgreen", las=1)
abline(h=2.0, col="black", lty=3, lwd=4)
#weiteren Wert in Betracht ziehen: Modalwert 
#columns
col_mode_b = apply(marke_bio, 2, modefunc)
#colnames
col_mode_b = t(col_mode_b)
colnames(col_mode_b) = numberofnamesHypothese3.1
#plot
barplot(col_mode_b, main = "Was verbinden die Kunden mit Robinson", ylab = "Bedeutung", col = "lightgreen", las=1)

```
***Antwort:* Die Attribute 'gesundheitsbewusste Ernährung', 'Hochwertige Nahrung', 'Naturbelassenheit', 'artgerechte Tierhaltung', 'Ausschluss von Gentechnik', 'geprüfte Qualität (Gütesiegel)' werden von den Gästen besonders mit der 'Marke' Bio assoziert.** 


Nun überlegen wir, welche Attribute der beiden Fragen einen Vergleich zulassen. Beispielsweise lässt sich das "umfassende Wohlfühlangebot" (3.2) mit "Wohlfühlen" (15.8) vergleichen. Nachfolgend sind einmal alle Attribute der Frage 3 mit den zugehörigen / vergleichbaren Attributen der Frage 15 aufgelistet.    

Gutes Preis- / Leistungsverhältnis: f3_1 = f15_16
Umfassendes Wohlfühlangebot: f3_2 = f15_8
Sportlichkeit: f3_3 = f15_6
Kinderfreundlichkeit: f3_4 = NA
Hochwertiges Speisenangebot: f3_5 = f15_4
Abwechslungsreiches Speisenangebot: f3_6 = f15_5
Einzigartigkeit: f3_7 = f15_7 (Individualität) ??
Hohe Qualität: f3_8 = NA
Gesundheit: f3_9 = f15_3
Im Trend: f3_10 = f15_9
Genuss: f3_11 = f15_12    

Diese vergleichbaren Daten werden nun zusammengeführt und in einem Säulendiagramm miteinander verglichen, um beurteilen zu können, in welchen Bereichen Bionahrung tatsächlich das Potential hat, die Marke Robinson zu stärken um so im Endeffekt dann eine allgemeine Aussage treffen zu können. 
```{r}
# vergleichsdaten zusammenführen 
vergleichs_matrix = cbind(col_means_r[1],col_means_b[16], 
                          col_means_r[2], col_means_b[8], 
                          col_means_r[3], col_means_b[6],
                          col_means_r[5], col_means_b[4],
                          col_means_r[6], col_means_b[5],
                          col_means_r[7], col_means_b[7],
                          col_means_r[9], col_means_b[3],
                          col_means_r[10], col_means_b[9],
                          col_means_r[11], col_means_b[12])
#colnames 
NamenTabelleHypothese3.2 = c('Preis- / Leistungsverhältnis',
                 'Umfassendes Wohlfühlangebot',
                 'Sportlichkeit',
                 'Hochwertiges Speisenangebot',
                 'Abwechslungsreiches Speisenangebot',
                 'Einzigartigkeit',
                 'Gesundheit',
                 'Im Trend',
                 'Genuss')
numberofnamesHypothese3.2<-c("1","2","3","4","5","6","7","8","9")

#colnames vergeben
vergleichs_matrix_dt = as.matrix(vergleichs_matrix)
colnames(vergleichs_matrix_dt) = c("1","1","2","2","3","3","4","4","5","5","6","6","7","7","8","8","9","9")
as.data.frame(NamenTabelleHypothese3.2, numberofnamesHypothese3.2)
#vergleichsmatrix plotten
barplot(vergleichs_matrix_dt, main = "Vergleich zwischen Bio und Robinson Club", ylab = "Bedeutung", col = c("lightgreen",'black'), beside = TRUE, las=1, cex.axis=.75, cex.names=.75)

```

Nachdem wir die Vergleichsdaten nun bereits einmal visuell verglichen haben, wird nun noch je Vergleichspaar ein T-Test durchgeführt, um zu schauen, ob jeweils ein signifikanter Verbesserungsbedarf besteht. 

```{r}
# Data Table erstellen
marke_robinson_dt = as.data.frame(marke_robinson)
marke_bio_dt = as.data.frame(marke_bio)
# T-Test von den Paarvergleichen
# T-Test Preis- / Leistungsverhältnis
t.test(marke_robinson_dt$V1,marke_bio_dt$V16)
# T-Test Umfassendes Wohlfühlangebot
t.test(marke_robinson_dt$V2,marke_bio_dt$V8)
# T-Test Sportlichkeit
t.test(marke_robinson_dt$V3,marke_bio_dt$V6)
# T-Test Hochwertiges Speisenangebot
t.test(marke_robinson_dt$V5,marke_bio_dt$V4)
# T-Test Abwechslungsreiches Speisenangebot
t.test(marke_robinson_dt$V6,marke_bio_dt$V5)
# Einzigartigkeit
t.test(marke_robinson_dt$V7, marke_bio_dt$V7)
# T-Test Gesundheit
t.test(marke_robinson_dt$V9,marke_bio_dt$V3)
# T-Test Im Trend --> p.value > alpha 
t.test(marke_robinson_dt$V10,marke_bio_dt$V9)
# T-Test Genuss
t.test(marke_robinson_dt$V11,marke_bio_dt$V12)
```
***Ergebnis:* Bis auf das 'abwechslungsreiche Speisenangebot' bestehen in allen Bereichen signifikante Verbesserungspotentiale. Dementsprechend hat Bionahrung das Potential die Marke Robinson zu stärken.**





# Fazit

**Ist die Einführung von Bionahrung beim Robinson Club aus betriebswirtschaftlicher Sicht strategisch sinvoll?**  

Im Rahmen des Fazits soll nun ein Rückbezug zu der eingangs erläuterten Zielformulierung hergestellt werden.  
Die Fragen und Hypothesen haben hier folgende Ergebnisse hervorgebracht.   
Es ist zunächst einmal deutlich geworden, dass das Essen einen hohen Stellenwert für die Gäste hat und Bionahrung generell als wichtig und erwünscht erachtet wird.   
Darüber hinaus haben wir uns mit der weiteren Analyse von Bionahrung beschäftigt. So haben wir herausgestellt, dass man sich besonders auf bestimmte Elemente von Bionahrung, wie "Frische und Reife", "Gesundheitsbewusste Ernährung", "Hochwertige Nahrung", "Naturbelassenheit", "Artgerechte Tierhaltung", "Ausschluss von Gentechnik" und "Geprüfte Qualität" konzentrieren sollte.   
Es ist zudem deutlich geworden, dass die Gäste zwar grundsätzlich bereit sind einen Aufschlag für Bionahrung zu leisten, aber gleichzeitig auch sehr preissensibel sind, was bedeutet, dass der Aufschlag sehr gering gehalten werden sollte.  
Außerdem ist das Angebot von Bionahrung eher bei Lebensmittelarten wie Gemüse & Salat, Obst und Fleischprodukten wichtig und es sollten vor allem die Biosiegel "Demeter", "Bioland" und "Bio" eingesetzt werden.  
Des Weiteren hat eine Analyse der Zielgruppe ergeben, für wen Bionahrung besonders wichtig ist und auch den Beweis geliefert, dass Personen mit Kindern Bionahrung nicht eher bevorzugen als Personen ohne Kinder.  
Diese Ergebnisse werden durch die aussagekräftige Hypothese (3) ergänzt. Unsere Analyse hat ergeben, dass Bionahrung das Potential besitzt, die Marke Robinson zu stärken.    
*Somit empfehlen wir dem Robinson-Club eine Einführung von Bionahrung in den oben genannten Lebensmittelkategorien. Der Fokus sollte hierbei unbedingt auf einem geringen Aufschlag, dem Einsatz bekannter Bio-Siegel und dem Herausstellen der wichtigsten Elemente von Bionahrung liegen.*
