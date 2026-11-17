# 🎲 Spieleabend

Kleine Web-App (PWA) für Lotta und Paul: Spiele zu zweit tracken – Siege, Punkte und Statistiken.
Läuft ohne eigenen Server über GitHub Pages und lässt sich aufs Handy installieren.

**App:** https://lottaspieleabend-wq.github.io/Spieleabend/

## Loslegen (einmalig, jede Person auf ihrem Handy)
1. Die App-Adresse oben in **Safari** öffnen.
2. Teilen-Symbol → **„Zum Home-Bildschirm“**. Ab jetzt die App über das Icon starten (Vollbild, offline nutzbar).
3. In der App unten auf **Mehr**, E-Mail-Adresse und ein eigenes Passwort (mind. 6 Zeichen) eingeben und **„Neues Konto erstellen“** tippen. Später reicht „Anmelden“.
4. **Eine** Person (z. B. Lotta) tippt unter *Mehr → Gemeinsam spielen* auf **„Gruppe erstellen“** und schickt den 6-stelligen Einladungscode an die andere Person (7 Tage gültig).
5. Die andere Person gibt den Code unter *Mehr → Gemeinsam spielen* ins Feld „Einladungscode“ ein und tippt **„Beitreten“**. Fertig – ab jetzt seht ihr beide dieselben geteilten Spiele und Partien.

Android: Chrome → Menü → „App installieren“.

## Features
- Spiele anlegen mit Emoji und Wertungsart:
  - **Höchste Punkte gewinnen** (z. B. Kniffel, Qwixx)
  - **Wenigste Punkte gewinnen** (z. B. Skyjo, Golf)
  - **Verlierer bekommt Minuspunkte** (z. B. Rummikub – optional bekommt der Gewinner sie als Plus)
  - **Nur Sieg / Niederlage**
- Partien eintragen, Gewinner wird automatisch bestimmt
- Verlauf mit Filter pro Spiel
- Statistik: Gesamtstand, Siegquote, Serien, Form, pro Spiel Ø-Punkte, Bestwert, Bilanz, Punkteverlauf
- Spielernamen und Farben anpassbar
- Backup als JSON exportieren/importieren
- Hell/Dunkel automatisch, offline nutzbar
- Synchronisierung zwischen beiden Handys (Konto mit E-Mail + Passwort)

## So funktioniert die Synchronisierung
- **Privat ist Standard:** Eigene Spiele und Partien sind nur für einen selbst sichtbar.
- **Gezielt teilen:** Nach dem Beitritt zur Gruppe lässt sich jedes Spiel über ✎ → *Sichtbarkeit* teilen (👥) oder privat halten (🔒). Partien wandern beim Umschalten mit.
- **Offline:** Eintragen geht auch ohne Netz, die Änderungen werden beim nächsten Verbindungsaufbau übertragen. Das Wolken-Symbol oben zeigt den Status (grün = synchron, orange = wird übertragen, grau = offline).
- **Konflikte:** Jede Partie ist ein eigener Eintrag. Gleichzeitige Einträge auf zwei Handys kollidieren nie.
- **Passwort vergessen:** Auf der Anmeldeseite „Passwort vergessen“ – es kommt eine E-Mail zum Zurücksetzen.

## Technik und Verwaltung
- Reines HTML/CSS/JavaScript ohne Build. Hosting: GitHub Pages (dieses Repo, Branch `main`, Ordner root).
- Daten: Firebase-Projekt `spieleabend-b203b` (Authentication mit E-Mail/Passwort, Firestore in Europa `eur3`, kostenloser Spark-Tarif).
- Verwaltet über das Google-Konto, das zu diesem GitHub-Konto gehört: [console.firebase.google.com](https://console.firebase.google.com).
- [`config.js`](config.js): Firebase-Konfiguration. Die Werte sind nicht geheim, sie benennen nur das Projekt. Geschützt werden die Daten durch Anmeldung und die Sicherheitsregeln in [`firestore.rules`](firestore.rules) (in der Firebase-Konsole unter *Firestore → Regeln* veröffentlicht).
- Firebase JS SDK 12.19.0 ist lokal gebündelt in `vendor/firebase.js`.

**Änderungen am Code:** Bei jeder Änderung `APP_VERSION` in `app.js` und `VERSION` in `sw.js` gemeinsam hochzählen, sonst lädt die installierte App das Update nicht. Nach dem Push ist die neue Version nach 1–2 Minuten live.

**Firebase-Bundle neu bauen** (nur bei SDK-Update nötig, immer in einem leeren Ordner – Reste alter Versionen erzeugen sonst doppelte Firebase-Kerne und den Fehler „Component auth has not been registered yet“):
```bash
npm i firebase@12.19.0 esbuild
npx esbuild vendor/firebase-entry.js --bundle --format=esm --minify --outfile=vendor/firebase.js
```

## Hinweise
- Ohne Konto liegen die Daten nur im Speicher des jeweiligen Geräts. Mit Konto sind sie in Firebase gesichert; ein gelegentliches JSON-Backup (*Mehr*) schadet trotzdem nicht.
