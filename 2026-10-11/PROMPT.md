# Prompt — „Welche Inhalte der 3. Klasse lassen sich mit R effizienter lösen?"

> Hausübung PMM (Prof. Graf), Kohorte 4AHWIT-y, Lektion 2026-10-09 „opencode".
> Verfasst mit OpenCode. Der Prompt ist bewusst so gebaut, dass die Antwort
> prüfbar, thematisch vollständig und auf den echten Lehrplan (KM5 + KM6)
> zugeschnitten ist.

---

## Rolle

Du bist erfahrener Dozent für **Statistik und R** an einer HTL und kennst den
Lehrplan **Prozessmanagement (PMM)** im Detail.

## Kontext

Im **III. Jahrgang** (PMM, Wirtschaftsingenieure) werden die Kompetenzmodule
**KM5** und **KM6** unterrichtet:

- **KM5 (5. Semester):** „Statistische Grundlagen der Qualitätssicherung,
  Parameter statistischer Verteilungen und ihre Schätzwerte."
- **KM6 (6. Semester):** „Zufallsstreu- und Vertrauensbereiche, Auswertung und
  Darstellung von Prüfergebnissen, Lebensdauerverteilungen."

Der Unterricht ist klassisch „händisch" geprägt: Tabellenwerke,
Formelsammlung, Taschenrechner, Excel, Plot auf Papier.

## Auftrag

Beantworte folgende Frage: **Welche Inhalte der 3. Klasse (PMM, KM5 + KM6)
lassen sich mit R effizienter lösen als mit den herkömmlichen Mitteln?**

## Anforderungen an die Antwort

1. Gliedere nach den **fünf Themenclustern** des Lehrplans:
   (a) Wahrscheinlichkeitsverteilungen, (b) Parameter & Schätzwerte,
   (c) Zufallsstreu- & Vertrauensbereiche, (d) Auswertung & Darstellung von
   Prüfergebnissen, (e) Lebensdauerverteilungen.
2. Nenne zu jedem Cluster **konkret**, was händisch mühsam ist und **wie** R es
   löst — mit **Paket- und Funktionsnamen** (z. B. `dbinom`, `pnorm`, `infer`,
   `t.test`, `qchisq`, `ggplot2`, `fitdistrplus`) und **kurzen R-Snippets**.
3. Belege den Effizienzgewinn (Reproduzierbarkeit, Genauigkeit,
   Automatisierung, Visualisierung, Simulation) — **keine** blosse
   Funktionsliste.
4. Nenne **Grenzen**: Wo hilft R nicht, bzw. wo bleiben Verständnis und
   Kopfrechnen nötig (Prüfungssituation, Verteilungsannahmen, Interpretation)?
5. Fasse das Ergebnis in einer **Vergleichstabelle** (Thema · händisch · mit R)
   und einem **Fazit** zusammen.
6. Zielgruppe: Schüler:innen der 4. Klasse — **Schulniveau**, keine
   Universitäts-Mathematik.

## Ausgabeformat

Reines **Markdown**, deutschsprachig, Überschriften als `##`, R-Code in
Codeblöcken mit Sprachkennung `r`.
