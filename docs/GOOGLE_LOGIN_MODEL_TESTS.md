# Xtree: Google Login und automatische Modelltests

## Ziel
- Google-Login als Identität für Xtree (Firebase Authentication).
- Nutzerbezogene Verbindungen und Modell-Teststatus.
- Modelltests, die Erfolg/Fehler und verständliche Fehlermeldungen in der Oberfläche anzeigen.
- Fallback: bei Fehlern ein anderes freigegebenes Modell versuchen.

## Grenzen
Google Login bei Xtree meldet den Nutzer nicht automatisch bei ChatGPT, Claude, Gemini, DeepSeek oder anderen Anbietern an. Dafür sind separate Anbieterfreigaben/OAuth-Flows oder ein aggregierter Anbieter (z. B. OpenRouter) notwendig. Keine Passwörter anderer Anbieter sammeln oder simulieren.

## Architektur (für den Implementierungsschritt)
1. Firebase Authentication mit Google Provider.
2. Serverseitige Verbindungstoken (OAuth/Provider-spezifisch), nicht im GitHub-Frontend oder localStorage.
3. Backend/Cloud Function führt Modelltests durch und meldet Ergebnisse (Modell, Status, Fehler, Zeitpunkt).
4. Oberfläche zeigt getestete, funktionierende Modelle an; fehlgeschlagene Modelle bleiben mit Fehlergrund sichtbar.
5. Vor dem Test oder dem ersten Zugriff auf einen neuen Anbieter wird dessen Verbindung autorisiert.
6. Nutzer kann Verbindungen jederzeit trennen.

## Erforderliche Konfiguration
- Firebase-Projekt, Web-App-Konfiguration, Google Sign-in Provider aktivieren.
- Backend/Cloud Functions mit geschützter Speicherung von Tokens.
- OAuth-Client und erlaubte Redirect-URLs für jeden direkt integrierten Anbieter, sofern dessen OAuth verfügbar ist.
- Anbieter-API- und OAuth-Richtlinien beachten; ein Google-Konto kann nicht stellvertretend andere Anbieter authentifizieren.

## Sicherheit
- Keine API-Schlüssel oder OAuth-Token in GitHub commits.
- Keine Tokens im Browser localStorage.
- Modelltests dürfen nur auf explizit freigegebenen Verbindungen ausgeführt werden.
