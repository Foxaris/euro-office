# Foxaris-Theme

Bringt den Editor in die Farben und das Zeichen von [Foxaris](https://foxaris.com).
Gebaut von `.github/workflows/foxaris-image.yml`; das Ergebnis liegt als Abbild unter
`ghcr.io/foxaris/euro-office`.

## Die Entscheidung dahinter

Wir liefern **genau ein Theme** aus: „Modern Hell" (`theme-white`) – das, was das
Projekt selbst als helle Vorgabe führt (`themeinit.js`, `DEFAULT_LIGHT_THEME_ID`).
Es hat eine helle Kopfleiste, größere Symbole und eine luftigere Werkzeugleiste
als das alte `theme-light`.

Unser Orange ist darin **Akzent, kein Band**: Unterstrich des aktiven Reiters,
Hauptschaltfläche in Dialogen, Fokusrahmen, ausgewählte Vorschau. Die Kopfleiste
bleibt hell.

## Was drin steckt

| Datei | Wofür |
|---|---|
| `meta/config.json` | Name, Herausgeber, Herkunftshinweis, Dateinamen der Logos |
| `assets/img/header/foxaris-fox.svg` | Fuchskopf farbig – Kopfleiste und Ladebild |
| `assets/img/header/foxaris-fox-white.svg` | Fuchskopf weiß – für dunkle Flächen |
| `assets/img/about/*` | dieselben beiden für den „Über"-Dialog |
| `assets/less/overrides/colors.less` | die Akzentfarben – das Herzstück |
| `assets/less/overrides/header.less` | Fuchs statt Schriftzug in der Kopfleiste |
| `assets/less/overrides/about.less` | Logo im „Über"-Dialog |
| `assets/less/overrides/mobile-overrides.less` | Markenfarbe mobil, Logostreifen aus |

Die weißen Fassungen sind derzeit unbenutzt – sie greifen nur in dunklen Themes,
und die liefern wir nicht aus. Sie bleiben liegen, damit ein späterer dunkler
Stand nichts nachzuzeichnen hat.

## Zwei Eingriffe außerhalb dieses Ordners

Der Branch war bis dahin rein additiv. Für „ein Theme, keine Auswahl" reichte das
nicht, weil der Editor beides nicht über die Konfiguration anbietet:

**`apps/common/main/lib/controller/Themes.js`** – `available()` gibt `false` zurück.
Der Editor entfernt daraufhin von sich aus Gruppe und Trenner aus dem
Ansicht-Reiter (`ViewTab.js`), in allen Editoren. Der Mechanismus ist vorhanden,
er war aus der Konfiguration nur nicht erreichbar: Einziger Aufrufer von
`setAvailable()` ist eine Windows-XP-Prüfung im Desktop-Programm.

**`apps/common/main/lib/util/htmlutils.js`** – der Vorbehalt `!window.uitheme.id`
ist raus, damit ein vom Integrator vorgegebenes Theme gegen eine gespeicherte Wahl
gewinnt. `themeinit.js` läuft vorher und setzt die Kennung aus dem Browserspeicher;
ohne diese Änderung bliebe jede früher getroffene Wahl bestehen – und ohne sichtbare
Auswahl käme niemand mehr davon los.

Beide Stellen sind je eine Zeile. Bei Versionssprüngen können sie einen Konflikt
geben; beide stehen kommentiert im Quelltext.

## Lokal ausprobieren

Für eine schnelle Farbprobe braucht es keinen Bau – die Farben sind am Ende
gewöhnliche CSS-Variablen. Im laufenden Container:

```bash
docker exec -u root <container> sh -c 'cat >> /var/www/euro-office/documentserver/web-apps/apps/documenteditor/main/app.css' < assets/less/overrides/colors.less
```

Danach den Editor neu laden. Für alles Weitere (Logos, „Über"-Dialog, mobil, die
beiden Eingriffe oben) führt kein Weg am Bau vorbei.
