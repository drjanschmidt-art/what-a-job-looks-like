# Ein Tag in dieser Stelle – Workshop-Tool

Teilnehmende fügen eine Stellenanzeige ein (oder laden PDF/Word/TXT), wählen ein Arbeitsumfeld und einen Tagestyp und lesen einen erzählten Arbeitstag in der Ich-Perspektive. Danach lässt sich dieselbe Anzeige in anderen Umfeldern durchspielen; alle Ergebnisse bleiben zum Vergleichen stehen.

## Dateien

- `index.html` – die Webseite
- `api/generate.js` – Server-Funktion, ruft Mistral mit deinem Schlüssel auf (der Schlüssel bleibt auf dem Server)
- `vercel.json`, `package.json` – Konfiguration für Vercel

## 1. Mistral-Schlüssel anlegen

1. Auf https://console.mistral.ai registrieren (Telefonnummer wird zur Verifizierung abgefragt).
2. Einen Plan wählen: **Experiment** (kostenlos, mit niedrigen Ratenlimits; Eingaben dürfen dann für das Training von Mistral genutzt werden) oder **Scale** (Bezahlung nach Verbrauch, keine Trainingsnutzung). Für den Workshop mit 12 Personen empfiehlt sich Scale: Das kostenlose Limit erlaubt sehr wenige Anfragen pro Sekunde, wenn alle gleichzeitig klicken, kommt es zu Wartemeldungen.
3. Unter **API Keys** auf „Create new key“ klicken, Namen vergeben (z. B. „Workshop Tag im Job“), optional ein Ablaufdatum setzen.
4. Schlüssel sofort kopieren, er wird nur einmal angezeigt. Nicht in Mails oder Chats weitergeben.

## 2. Auf Vercel bringen

**Variante A – über GitHub (ohne Kommandozeile):**
1. Auf github.com ein neues (privates) Repository anlegen und die vier Dateien hochladen („Add file → Upload files“, den Ordner `api` mit `generate.js` beibehalten).
2. In Vercel „Add New → Project“, das Repository importieren, Framework „Other“ lassen, „Deploy“.

**Variante B – mit der Vercel-CLI:**
`npm i -g vercel`, dann im entpackten Ordner `vercel` und anschließend `vercel --prod` ausführen.

## 3. Umgebungsvariablen in Vercel setzen

Im Projekt: **Settings → Environment Variables**

| Name | Wert | Pflicht |
|---|---|---|
| `MISTRAL_API_KEY` | dein Schlüssel aus Schritt 1 | ja |
| `ACCESS_CODE` | ein Workshop-Code, z. B. `herbst26` | empfohlen |
| `MISTRAL_MODEL` | z. B. `mistral-small-latest` (günstiger) | nein, Standard ist `mistral-medium-latest` |

Danach unter **Deployments** beim letzten Deployment „Redeploy“ wählen – Variablen wirken erst nach einem neuen Deployment.

Mit gesetztem `ACCESS_CODE` fragt die Seite nach dem Code; ohne Code funktioniert sie nicht. So kann niemand außerhalb des Workshops dein Guthaben verbrauchen, falls der Link weitergegeben wird. Nach dem Workshop den Code ändern oder den Schlüssel in der Mistral-Konsole löschen.

## 4. Testen

Seite öffnen. Erscheint oben ein blauer Hinweis, fehlt der Schlüssel oder das Redeploy. Mit einer echten Anzeige einmal „Wie in der Anzeige“ und einmal ein anderes Umfeld durchspielen.

## Kosten im Blick behalten

Jeder Durchlauf ist auf rund 1.500 Ausgabe-Tokens begrenzt. Aktuelle Preise stehen auf mistral.ai/pricing; in der Mistral-Konsole lässt sich der Verbrauch einsehen.
