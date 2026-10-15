# Antwort — Welche Inhalte der 3. Klasse (PMM) lassen sich mit R effizienter lösen?

Der III. Jahrgang umfasst **KM5** (Wahrscheinlichkeitsverteilungen, Parameter
& Schätzwerte) und **KM6** (Zufallsstreu-/Vertrauensbereiche, Auswertung &
Darstellung von Prüfergebnissen, Lebensdauerverteilungen).

**Kernthese:** R ersetzt nicht das Verständnis — aber die **Rechen-,
Tabellen- und Zeichenarbeit** fällt weg. Was händisch pro Aufgabe Minuten
kostet, erledigt R in einer Zeile, exakt, reproduzierbar und für beliebig
viele Messwerte.

---

## 1. Wahrscheinlichkeitsverteilungen (KM5)

**Händisch mühsam:** Φ-Tabelle und Binomial-/Poisson-Tabellen nachschlagen,
Streubereiche über `P(X ≤ k)` mit Fallunterscheidungen (`<`, `≤`, `=`),
Standardisierung `Z = (X − μ)/σ`, Kombinatorik für die Binomialkoeffizienten,
Dichten von Hand zeichnen.

**Mit R:** die `d/p/q/r`-Familie — Dichte/Höhe (`d`), Verteilungsfunktion (`p`),
Quantil (`q`), Zufallszahlen (`r`) — für *jede* Verteilung einheitlich.

```r
# Binomialverteilung B(20, 0.3)
dbinom(5, 20, 0.3)       # P(X = 5)
pbinom(5, 20, 0.3)       # P(X <= 5)
1 - pbinom(5, 20, 0.3)   # P(X > 5)
qbinom(0.95, 20, 0.3)    # 95%-Quantil
rbinom(1000, 20, 0.3)    # 1000 simulierte Stichproben

# Normalverteilung N(100, 2): P(X > 103) – statt Standardisieren + Tabelle
pnorm(103, mean = 100, sd = 2, lower.tail = FALSE)
qnorm(0.975)             # z = 1,96 (kein Tabellennachschlagen)

# Hypergeometrisch (ohne Zurücklegen) und Poisson
phyper(2, m = 10, n = 40, k = 5)
dpois(3, lambda = 2)

# Verteilung zeichnen statt per Hand
curve(dnorm(x, 100, 2), from = 94, to = 106, main = "Normalverteilung N(100, 2)")
```

**Effizienzgewinn:** exakte (nicht gerundete) Werte, kein Tabellen-Nachschlagen,
beliebige `n`/`p`/`λ`, Quantile und Simulation sofort verfügbar.

## 2. Parameter & Schätzwerte (KM5)

**Händisch mühsam:** Mittelwert aufsummieren, Varianz mit Abweichungsquadraten,
Bessel-Korrektur (`n − 1`), mehrere Kennzahlen getrennt, Ausreißer suchen.

**Mit R:** alle Lage- und Streumaße in einem Aufruf; Simulation macht Konzepte
wie Erwartungstreue und das Gesetz der großen Zahlen sichtbar.

```r
x <- c(12.3, 12.5, 11.9, 12.8, 12.1, 12.4, 12.0)
summary(x)                      # Min, Q1, Median, Mittel, Q3, Max auf einmal
mean(x); sd(x); var(x)          # Lage und Streuung
IQR(x); quantile(x, c(.25, .75))
# Bessel sichtbar: var() teilt durch n-1, nicht durch n
c(var_mit_n = var(x) * (length(x) - 1) / length(x), var_mit_n_minus_1 = var(x))
# Gesetz der großen Zahlen (Monte-Carlo)
set.seed(1); mean(replicate(10000, mean(rnorm(30, 100, 2))))  # ~ 100
```

**Effizienzgewinn:** keine Additions-/Rundungsfehler, robuste Kennzahlen
(Median, IQR) gleich mitgeliefert, „warum erwartungstreu?" wird per Simulation
überprüfbar statt nur behauptet.

## 3. Zufallsstreu- & Vertrauensbereiche (KM6)

**Händisch mühsam:** Standardfehler `σ/√n`, kritische Werte `z`/`t` aus Tabellen,
Freiheitsgrade, mehrere Konfidenzniveaus, χ²-Bereich für die Varianz.

**Mit R:**

```r
set.seed(1); x <- rnorm(25, 100, 2)
# KI für μ bei bekanntem σ (z)
mean(x) + c(-1, 1) * qnorm(0.975) * 2 / sqrt(25)
# KI für μ bei unbekanntem σ (t) – liefert Intervall und Test in einem Schritt
t.test(x, conf.level = 0.95)$conf.int
qt(0.975, df = 24)                       # t-Wert statt Tabelle
# KI für die Varianz (χ²-Verteilung)
s2 <- var(x); df <- 24
c(unten = df * s2 / qchisq(0.975, df), oben = df * s2 / qchisq(0.025, df))
```

**Effizienzgewinn:** keine Tabellen, exakte `df`-abhängige Werte,
Konfidenzniveau als Parameter, `t.test` liefert Konfidenzintervall und Test
gleichzeitig; die Intervallbreite lässt sich experimentell über `n`/`σ`
untersuchen.

## 4. Auswertung & Darstellung von Prüfergebnissen (KM6)

**Händisch mühsam:** Histogramm mit Klassen einteilen und zeichnen, Boxplot und
QQ-Plot per Hand, Ausreißer nach der 1,5·IQR-Regel, Streudiagramme — pro
Prüfreihe wieder von vorn.

**Mit R:** `ggplot2` erzeugt alle Standarddarstellungen aus denselben Daten;
Quarto macht daraus einen Bericht (Text + Code + Grafik).

```r
library(ggplot2)
df <- data.frame(messwert = rnorm(200, 100, 2))

ggplot(df, aes(messwert)) +
  geom_histogram(bins = 20, fill = "lightgreen")     # Histogramm

ggplot(df, aes(x = "", y = messwert)) +
  geom_boxplot(fill = "lightblue")                   # Boxplot inkl. Ausreißer

ggplot(df, aes(sample = messwert)) +
  stat_qq() + stat_qq_line()                         # QQ-Plot
```

**Effizienzgewinn:** ein Skript = reproduzierbare, immer gleich formatierte
Grafik für beliebig viele Prüfreihen — genau „Auswertung und **Darstellung**
von Prüfergebnissen".

## 5. Lebensdauerverteilungen (KM6)

**Händisch mühsam:** Weibull-`F(t)`/`R(t)`/`λ(t)` zeichnen, MTBF/MTTF,
63,2 %-Punkt, und vor allem die **Anpassung** von β/η an echte Ausfallzeiten.

**Mit R:**

```r
# Weibull mit Formparameter β und Skalenparameter η (charakteristische Lebensdauer)
beta <- 1.8; eta <- 1000
pweibull(500, beta, eta)               # F(500) = P(X <= 500)
1 - pweibull(500, beta, eta)           # R(t) = P(X > t)
curve(1 - pweibull(x, beta, eta), from = 0, to = 3000, ylim = c(0, 1),
      main = "Zuverlässigkeit R(t)")
eta * gamma(1 + 1 / beta)              # MTTF

# Exponentialfall (β = 1): gedächtnislos, konstante Ausfallrate
lambda <- 0.001; 1 / lambda            # MTTF = 1/λ

# β/η aus echten Daten schätzen und Anpassung prüfen
# fitdistrplus::fitdist(zeiten, "weibull")
```

**Effizienzgewinn:** Ausfallwahrscheinlichkeiten und Quantile exakt, `R(t)`- und
`λ(t)`-Kurven sofort, Parameter frei variierbar; `fitdistrplus` schätzt β/η aus
Daten und prüft die Anpassung — händisch praktisch nicht durchführbar.

---

## Vergleichstabelle

| Thema (Cluster) | Händisch | Mit R |
|---|---|---|
| Wahrscheinlichkeitsverteilungen | Tabellen, Standardisieren, Kombinatorik | `d/p/q/r`-Familie, `curve()` — exakt, jede Verteilung |
| Parameter & Schätzwerte | Summen, Abweichungsquadrate, Bessel | `summary()`, `mean()`, `sd()`, `IQR()`, Simulation |
| Streu-/Vertrauensbereiche | SE + z/t/χ²-Tabellen + df | `t.test()$conf.int`, `qnorm()`, `qt()`, `qchisq()` |
| Darstellung von Prüfergebnissen | Histogramm/Boxplot/QQ von Hand | `ggplot2`, Quarto — reproduzierbar |
| Lebensdauerverteilungen | `R(t)`, `λ(t)`, β/η-Fit kaum machbar | `pweibull()`, `MTTF = η·Γ(1+1/β)`, `fitdistrplus` |

## Fazit

Am grössten ist der Gewinn, wo **viel gerechnet oder gezeichnet** wird:
Verteilungsrechnungen, Konfidenzintervalle, Auswertung/Darstellung von
Prüfergebnissen und Lebensdauerkurven. R macht daraus wenige Zeilen Code, die
man **wiederverwenden und dokumentieren** kann.

## Grenzen — wo R (allein) nicht hilft

- **Verstehen:** Welche Verteilung passt? Was bedeutet „Gedächtnislosigkeit",
  „Erwartungstreue", „Konfidenzniveau"? Das bleibt Denkarbeit.
- **Interpretation:** R liefert eine Zahl, nicht die fachliche Bewertung
  („Prozess in Ordnung?" ist keine Rechenaufgabe).
- **Annahmen:** Falsche Modellwahl → falsches Ergebnis („garbage in, garbage
  out").
- **Prüfungssituation:** Ohne Laptop bleibt das Rechnen mit Tabelle/TR nötig —
  R ist Werkzeug, nicht Ersatz für die Grundlagen.

> **Fazit:** R ist beim **Rechen- und Darstellungsaufwand** klar effizienter;
> die **statistische Denkweise** muss weiterhin im Kopf entstehen.

---

*Erstellt mit OpenCode · Hausübung PMM, Kohorte 4AHWIT-y.*
