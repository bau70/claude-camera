# 📷 Camera Live View — skill per Claude

Una skill per [Claude](https://claude.ai) che ti permette di **usare la fotocamera dell'iPhone per mandare foto a Claude** e farle guardare e commentare, anche in modalità **vista live** (un fotogramma ogni pochi secondi).

> *English summary:* a Claude skill that lets you snap photos (or stream a frame every few seconds) from your iPhone camera and have Claude look at them. It works through a private web page ("iPhone Camera") that Claude publishes in your own claude.ai account.

---

## Cosa puoi fare

- **Scattare una foto** dall'iPhone e chiedere a Claude "cosa vedi?"
- **Inviare foto dalla galleria**, anche più di una, con una nota (es. "leggi lo scontrino")
- **Vista live**: la pagina invia un fotogramma ogni 3, 5 o 10 secondi e Claude descrive cosa inquadri in quel momento
- **Dal Mac**: usare l'iPhone come webcam (Continuity Camera) e scattare da lì

## Come funziona

La skill ha due parti:

| File | A cosa serve |
|---|---|
| `camera-live-view/SKILL.md` | Le istruzioni per Claude: come creare la pagina, dove trovare le foto, come guardarle |
| `camera-live-view/assets/iphone-camera.html` | La pagina "iPhone Camera" che si apre sul telefono |

Il percorso di una foto:

```
iPhone                        claude.ai (privato)                 Claude
──────                        ───────────────────                 ──────
📷 Scatta foto  ──upload──▶   pagina "iPhone Camera"
                              ├─ assets: il file JPG
                              └─ database: riga in "photos"  ◀── legge le foto nuove,
                                                                  le scarica e le guarda
```

1. **Primo utilizzo.** Claude pubblica la pagina `iphone-camera.html` come *artifact* privato nel tuo account claude.ai, con tre permessi: `assets` (salvare i file), `db` (un piccolo database) e `user` (sapere chi scatta).
2. **Scatti.** Apri la pagina sull'iPhone e premi *Scatta foto*. La foto viene ridotta a max 1600 px, caricata tra gli asset della pagina e registrata nella collezione `photos` del database (`assetId`, ora, nota, `seen: false`).
3. **Claude guarda.** Quando scrivi "cosa vedi?", Claude legge le foto più recenti non ancora viste, scarica l'immagine, la analizza e la segna come vista (`seen: true`).
4. **Vista live.** Il pulsante *Avvia live* usa la fotocamera del browser e salva ogni fotogramma nel documento `live/current`. Ogni nuovo fotogramma sostituisce il precedente, così lo spazio non si riempie.

Tutto resta nella **tua** pagina privata: nessun server esterno.

## Installazione

1. Scarica la repository (**Code → Download ZIP**) ed estraila.
2. Comprimi la cartella `camera-live-view` in uno ZIP (su Mac: tasto destro → *Comprimi*). Lo ZIP deve contenere la cartella con dentro `SKILL.md`.
3. Su claude.ai vai su **Personalizza → Skill**, clicca **+** → **Crea skill** → **Carica una skill** e scegli lo ZIP.
4. Attiva la skill.

Requisiti: un account claude.ai con le skill e gli artifact pubblicati attivi (es. Claude desktop / Cowork).

## Uso

1. Scrivi a Claude: *"voglio mandarti una foto con l'iPhone"*.
2. La prima volta Claude crea la pagina **iPhone Camera** e ti dice di aprirla sul telefono. Sul Mac la pagina mostra un **codice QR** da inquadrare con l'iPhone; in alternativa la trovi tra gli artifact nell'app Claude per iPhone.
3. Sull'iPhone premi **📷 Scatta foto** (o **🖼️ Scegli dalla galleria**).
4. Torna nella chat e scrivi **"cosa vedi?"**.

Per la vista live: premi **▶︎ Avvia live**, poi chiedi a Claude "cosa vedi adesso?" tutte le volte che vuoi. Premi **■ Ferma live** quando hai finito.

## Domande frequenti

**Sul Mac "Scatta foto" apre il Finder.**
È normale: il pulsante usa la fotocamera del dispositivo su cui apri la pagina. Aprila sull'iPhone (codice QR) oppure usa la vista live e scegli l'iPhone come camera.

**La vista live non parte.**
Alcuni browser non permettono la fotocamera in diretta dentro claude.ai. *Scatta foto* funziona sempre.

**Le foto HEIC danno errore.**
Su iPhone: *Impostazioni → Fotocamera → Formati → Più compatibile*.

**Posso condividere la mia pagina con altri?**
Meglio di no: le loro foto finirebbero nella tua pagina. Condividi la skill; ognuno crea la propria pagina al primo utilizzo.

## Privacy

- La pagina è privata: la vedi solo tu, finché non la condividi.
- Claude ti avvisa prima di chiederti di accendere una fotocamera e non scatta mai da solo.
- Le foto si cancellano solo su tua richiesta.

## Alternative incluse nella skill

- Allegare una foto direttamente nell'app Claude per iPhone (**+ → Fotocamera**).
- Su macOS da terminale (es. Claude Code): `brew install imagesnap` e `imagesnap -w 1.5 foto.jpg`.

## Licenza

[MIT](LICENSE)
