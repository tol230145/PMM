# PMM

Diegos PMM Repo 
# Gesamte statistische Übersicht einer Messreihe
daten <- c(12.3, 12.5, 11.9, 12.8, 12.1, 12.4, 12.0)
summary(daten)
sd(daten) # Standardabweichung
# Boxplot und Histogramm in einer Ansicht
par(mfrow = c(1, 2))
boxplot(daten, main = "Toleranz-Boxplot", col = "lightblue")
hist(daten, main = "Häufigkeitsverteilung", col = "lightgreen")
# Normalverteilung: µ = 100, σ = 2. Wahrscheinlichkeit für Wert > 103
pnorm(103, mean = 100, sd = 2, lower.tail = FALSE)
# Stichprobenvergleich zweier Maschinen
maschine_A <- c(10.1, 10.2, 9.8, 10.0, 10.1)
maschine_B <- c(9.5, 9.7, 9.6, 9.4, 9.8)

temp <- c(180, 190, 200, 210, 220)
ausschuss <- c(5.2, 4.1, 3.0, 2.8, 1.9)

modell <- lm(ausschuss ~ temp)
summary(modell) # Zeigt R², Koeffizienten und p-Werte

