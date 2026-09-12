# retypeset-book

**Setzt Bücher von [einfachvorlesen.de](https://www.einfachvorlesen.de) neu —
für den Bildschirm, von dem tatsächlich vorgelesen wird.**

einfachvorlesen.de, ein kostenloser Dienst von Stiftung Lesen und Deutsche Bahn
Stiftung, stellt jeden Monat Kinderbücher als PDF bereit. Diese PDFs entstehen
vollautomatisch, und das sieht man ihnen an: Kapitelüberschriften landen allein
am Seitenfuß, jede Seite trägt Logo, Dienstzeile und Seitenzähler, Illustrationen
kleben linksbündig, und der A4-Satz macht die Schrift auf einem Tablet klein.

Lesbar ist das. Schön nicht — und es wird eine halbe Stunde am Stück vorgelesen.
Dieses Skript setzt die Bücher noch einmal, mit Typst.

## Was sich ändert

- **Seitenformat 18 × 24 cm statt A4.** Das ist exakt 3:4 und füllt ein 13"
  iPad Pro hochkant randlos. Weil man jede Seite ins Fenster einpasst, heißt
  eine kürzere Seite größere Schrift: rund 1,24× auf dem 13", 1,17× auf dem 11".
- **Kein Seitenschmuck.** Logo, „Ein Service von …" und der `5/20`-Zähler sagen
  einem Kind nichts und unterbrechen den, der vorliest.
- **Überschriften kleben an ihrem Text.** Kapitel beginnen auf neuer Seite, mit
  Inhaltsverzeichnis.
- **Luft, die man wiederfindet.** Zeilenabstand 1,50× und Absatzabstand 2,25×
  des Schriftgrads — die Werte, die WCAG 1.4.12 ansetzt; die Quelle bleibt mit
  1,43× knapp darunter. Kein Erstzeileneinzug: Wer vorliest, hebt den Blick zum
  Kind und sucht die Zeile danach wieder, und eine Lücke findet man dabei
  zuverlässiger als einen Einzug.
- **Flattersatz mit widerwilliger Trennung.** Gleichmäßige Wortabstände lesen
  sich bei Sehschwäche besser als Blocksatz, und ein getrenntes Wort lässt beim
  Vorlesen stolpern — die Trennung kostet deshalb 400%, Typst greift nur dazu,
  wenn eine Zeile sonst unschön kurz bliebe.
- **Illustrationen schwimmen** statt ein Loch zu lassen, und werden nie über
  ihre Größe in der Quelle hinaus vergrößert.
- **Bilderbücher behalten ihre Seiten.** Kommen auf ein Bild weniger als 7
  Zeilen Text, liefen schwimmende Bilder dem Text davon. Dann wird jede Seite
  der Quelle genau eine Seite, Bild und Text bleiben beisammen. Passt eine
  Seite nicht, werden die Bilder kleiner (bis 80 %), der Text nie.
- **Betonung bleibt.** Kursives und die Medium-Schnitte der Quelle — gerufene
  Wörter wie „Nein!" und „PING!" — überleben, weil beide sagen, wie ein Satz
  klingen soll.
- **Mehrteilige Bücher werden zusammengesetzt.** Längere Geschichten kommen als
  `-teil-1`, `-teil-2` … Übergib alle Teile auf einmal.

Gesetzt wird in **Atkinson Hyperlegible Next**, 14pt — der Schrift der Quelle,
entworfen für Menschen mit Sehschwäche. Sie liegt unter `fonts/` bei, weil die
in den Quell-PDFs eingebetteten Kopien nur die Zeichen enthalten, die das
jeweilige Buch gerade brauchte.

## Voraussetzungen

Vorhanden sein müssen, gleich auf welchem Weg installiert:

- Python 3.9 oder neuer
- [PyMuPDF](https://pymupdf.readthedocs.io), importierbar für dieses Python
- [Typst](https://typst.app), als Programm `typst` im `PATH`

## Benutzung

Gib dem Skript, was du gerade hast. Es erkennt am Pfad, was zu tun ist.

**Frische Downloads** — auslesen und setzen:

```bash
./retypeset-book.py downloads/moppi-und-moehre-teil-*.pdf
```

**Ein Arbeitsordner** — noch einmal setzen, nach einer Korrektur:

```bash
./retypeset-book.py work/moppi-und-moehre
```

`--preview` legt zusätzlich Stichprobenseiten als PNG ab, für den schnellen
Blick ohne PDF-Betrachter.

## Die Ordner

Vier Ordner neben dem Skript, durch die ein Buch wandert. Keiner ist
versioniert — für die Geschichten gibt es keine Lizenz zum Weitergeben.

| Ordner       | Inhalt                                                  |
| ------------ | ------------------------------------------------------- |
| `downloads/` | Was noch aussteht. Frisch geladene Quell-PDFs.          |
| `work/`      | Je Buch ein Ordner: `buch.json`, `buch.typ`, `bilder/`. |
| `processed/` | Ausgelesene Quellen. Das Original wird nie überschrieben. |
| `retypeset/` | Die fertigen Bücher.                                    |

Die Ordner legt das Skript selbst an; `downloads/` füllst du.

## `buch.json` ist der Punkt

Zwischen Auslesen und Setzen liegt eine Textdatei: eine schlichte Liste aus
Absätzen, Überschriften und Bildern. Was im fertigen PDF falsch aussieht — eine
Kapitelgrenze an der falschen Stelle, ein zerrissener Absatz, ein Bild, das drei
Seiten zu früh kommt — wird dort korrigiert. Danach den Arbeitsordner erneut
übergeben, beliebig oft; die Korrektur überlebt jeden Rebuild.

Bei Bilderbüchern steht dort zusätzlich `"picture_book": true`, und
`{"type": "pagebreak"}` markiert jede Seitengrenze der Quelle. Eine Marke zu
verschieben verschiebt die Seitengrenze; `false` setzt das Buch wie jedes andere
mit schwimmenden Bildern.

Deshalb läuft das Setzen auch gleich nach dem Auslesen durch: Ein Rezept will
niemand um seiner selbst willen ansehen. Der Grund, hineinzuschauen, ist etwas
im fertigen Buch — und das muss dafür erst existieren.

## Grenzen

**Das Werkzeug liest ausschließlich PDFs von einfachvorlesen.de.** Die ganze
Auslese hängt daran, dass dort die Schriftgröße allein sagt, was ein Element
ist: 24pt Titel, 16pt Überschrift, 14pt Fließtext, 10pt Seitenschmuck. Über
alle 25 vermessenen Bücher gilt das ausnahmslos — bei einem PDF aus anderer
Quelle gilt es nicht, und dabei käme ein Buch ganz ohne Text heraus. Das Skript
prüft deshalb zuerst, ob Atkinson Hyperlegible in der Quelle steckt, und
verweigert ein Buch, das keine Absätze ergibt. Ein Buch von woanders braucht
seine eigene Auslese.

Was das Skript nicht kann: Es korrigiert nichts an der Quelle zurück, und es
rät nicht. Eine 16pt-Zeile gilt nur dann als Kapitel, wenn es mindestens zwei
davon gibt und die erste im ersten Viertel des Textes steht — sonst ist es ein
Ausruf mitten in der Geschichte oder der Kopf eines Sachanhangs, und beides
wird an Ort und Stelle gesetzt.

## Verwandtes

Entstanden für die [Super Vorlese-App](https://github.com/DrDonik/super-vorlese-app),
mit der Großeltern und Enkelkinder über die Distanz gemeinsam dasselbe Buch
lesen. Die ausführliche Begründung der Entscheidungen oben steht dort als
[ADR 28](https://github.com/DrDonik/super-vorlese-app/blob/main/doc/adr/0028-books-are-retypeset-before-they-reach-the-shelf.md).

Eine Sache verbindet beide über die Repo-Grenze hinweg: Die App liest Coverbild
und Altersempfehlung aus Seite 1 des fertigen PDF. Wer hier die Titelseite
ändert, sieht bitte dort nach.

## Lizenz

Das Skript ist [gemeinfrei](https://unlicense.org/) (The Unlicense). Die
Schriften unter `fonts/` stehen unter der SIL Open Font License, siehe
`fonts/OFL.txt`. Die Bücher gehören ihren Verlagen und sind hier nicht enthalten.
