# Was das hier ist

Ein einzelnes Python-Skript, das PDFs von einfachvorlesen.de neu setzt, damit
sie sich auf einem iPad vorlesen lassen. Es entstand für die
[Super Vorlese-App](https://github.com/DrDonik/super-vorlese-app) und lebt
seit August 2026 für sich, weil es mit deren Code nie etwas zu tun hatte.

# Regeln

1. **Die Titelseite ist eine Schnittstelle.** Die Super Vorlese-App liest
   Coverbild und Altersempfehlung aus Seite 1 des fertigen PDF (ADR 29 dort).
   Sie erkennt beides daran, dass die Seite genau ein Rasterbild trägt und die
   Altersangabe als Text enthält. Wer an `render_typst` die Titelseite ändert,
   prüft das gegen die App — kaputt geht es dort still.
2. **Zahlen im Skript sind gemessen, nicht geraten.** Die Schriftgrößen-
   Signatur der Quelle, der Durchschuss, die Bildschwellen: jede Konstante hat
   einen Kommentar, der sagt, woher sie kommt. Wer eine ändert, misst neu und
   schreibt hin, woran.
3. **Kein Modell im Werkzeug.** Die Auslese ist deterministisch: die
   Schriftgröße sagt, was ein Element ist, und sonst entscheidet nichts.
   Kein Aufruf an ein Sprachmodell, kein Schlüssel, kein Dienst. Das ist
   die Zusage aus [ADR 37](https://github.com/DrDonik/super-vorlese-app/blob/main/doc/adr/0037-no-ai-joins-the-reading.md)
   der App, und sie ist mit umgezogen. Wer für ein Buch aus anderer Quelle
   von Hand ein `buch.json` mit einem Modell erzeugt, tut Handarbeit
   außerhalb dieses Repos — erlaubt, aber es wird kein Bestandteil des
   Werkzeugs.
4. **Nie direkt implementieren.** Erst den Plan und das Ergebnis für den, der
   vorliest, darstellen, dann auf ausdrückliche Zusage hin bauen.
5. **Geprüft wird an einer gesetzten Seite, die jemand ansieht** — es gibt
   keine Tests und keinen Linter, und das bleibt so. `--preview` rendert
   Stichprobenseiten als PNG genau dafür.
6. **Persönliches Werkzeug.** Ein Maintainer, alle Bücher lokal. Ältere
   `buch.json` dürfen brechen; Migrationspfade braucht es nicht.
7. **Kommentare und Ausgaben auf Deutsch**, Bezeichner im Code auf Englisch —
   so ist das Skript geschrieben.
