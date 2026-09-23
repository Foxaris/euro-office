# Foxaris-Theme

Bringt den Editor in die Farben und das Zeichen von [Foxaris](https://foxaris.com).
Gebaut wird er von `.github/workflows/foxaris-image.yml`; das Ergebnis liegt als
Abbild unter `ghcr.io/foxaris/euro-office`.

Dieser Ordner ist **additiv** – er verändert keine Datei des Projekts. Deshalb
steht er auf einem eigenen Branch, während `main` ein unberührter Spiegel bleibt.

## Was drin steckt

| Datei | Wofür |
|---|---|
| `meta/config.json` | Name, Herausgeber, Herkunftshinweis, Dateinamen der Logos |
| `assets/img/header/foxaris-fox.svg` | Fuchskopf farbig – für helle Flächen |
| `assets/img/header/foxaris-fox-white.svg` | Fuchskopf weiß – für die Kopfleiste |
| `assets/img/about/*` | dieselben beiden für den „Über"-Dialog |
| `assets/less/overrides/colors.less` | die Oberflächenfarben – das Herzstück |
| `assets/less/overrides/header.less` | Fuchs statt Schriftzug in der Kopfleiste |
| `assets/less/overrides/about.less` | Logo im „Über"-Dialog |
| `assets/less/overrides/mobile-overrides.less` | Markenfarbe mobil, Logostreifen aus |

## Die eine Kopplung, die man kennen muss

Der weiße Fuchs hat seine Binnenzeichnung in der Kopfleistenfarbe **ausgespart**
(`#ea580c`, siehe `overrides/colors.less`). Ein ganz weißer Fuchs verliert bei
20 Pixeln jede Zeichnung und wird zum Fleck; der farbige verschwindet auf Orange.

Ändern wir also das Orange, muss `foxaris-fox-white.svg` mit. Beide Dateien
liegen deshalb in diesem Ordner nebeneinander.

## Was der Editor nicht über diesen Ordner bekommt

Das helle Standard-Theme wird eingefärbt, die dunklen Themes und die neutralen
Varianten („Grau", „Weiß") bleiben unangetastet. Wer die im Editor unter
„Ansicht → Design" auswählt, will genau die.

## Lokal ausprobieren

Für eine schnelle Farbprobe braucht es keinen Bau – die Farben sind am Ende
gewöhnliche CSS-Variablen. Im laufenden Container:

```bash
docker exec -u root <container> sh -c 'cat >> /var/www/euro-office/documentserver/web-apps/apps/documenteditor/main/app.css' < assets/less/overrides/colors.less
```

Danach den Editor neu laden. Für alles Weitere (Logos, „Über"-Dialog, mobil)
führt kein Weg am Bau vorbei.
