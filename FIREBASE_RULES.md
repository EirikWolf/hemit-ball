# Firebase Database Regler for Hemit Ball

## Sett opp database-regler

1. Gå til Firebase Console: https://console.firebase.google.com
2. Velg prosjektet "hemit-ball"
3. Klikk på "Realtime Database" i venstre meny
4. Klikk på "Rules" fanen
5. Erstatt innholdet med reglene nedenfor
6. Klikk "Publish"

## Database Rules (kopier dette):

```json
{
  "rules": {
    "highscores": {
      // Alle kan lese highscores
      ".read": true,
      
      // Alle kan legge til nye scores
      ".write": true,
      
      "$scoreId": {
        // Valider strukturen på hver score
        ".validate": "newData.hasChildren(['name', 'score', 'level', 'timestamp'])",
        
        "name": {
          ".validate": "newData.isString() && newData.val().length <= 20"
        },
        "score": {
          ".validate": "newData.isNumber() && newData.val() >= 0 && newData.val() <= 10000000"
        },
        "level": {
          ".validate": "newData.isNumber() && newData.val() >= 1 && newData.val() <= 100"
        },
        "timestamp": {
          ".validate": "newData.isNumber()"
        }
      }
    }
  }
}
```

## Hva gjør disse reglene?

- ✅ Alle kan lese highscore-listen
- ✅ Alle kan legge til nye scores
- ✅ Scores må ha riktig format (navn, score, level, timestamp)
- ✅ Navn maks 20 tegn
- ✅ Score må være mellom 0 og 10.000.000
- ✅ Level må være mellom 1 og 100
- 🛡️ Hindrer ugyldige data fra å bli lagret

## Viktig: For produksjon

For ekstra sikkerhet kan du senere legge til:
- Rate limiting (maks X scores per bruker per time)
- IP-logging
- Mer avansert anti-juks validering

Men for et enkelt spill er reglene over tilstrekkelige!
