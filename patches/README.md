# Patches

Zwei Eingriffe in Dateien des Projekts, die unser Theme braucht und die sich nicht
über die Konfiguration erreichen lassen. Sie liegen hier als Patch und **nicht** als
geänderte Dateien im Branch – aus zwei Gründen:

1. Der Ablauf baut nicht diesen Branch, sondern einen frischen Auscheck von
   `Euro-Office/web-apps` beim gepinnten Release-Commit. Geänderte Dateien hier
   würden ihn nie erreichen.
2. `git apply` scheitert laut, wenn der Kontext nicht mehr passt. Ein Versionssprung,
   der eine dieser Stellen umbaut, macht den Bau rot – statt still ein Abbild ohne
   unsere Eingriffe zu erzeugen.

| Patch | Was er tut |
|---|---|
| `0001-keine-theme-auswahl.patch` | `Themes.available()` gibt `false` zurück. Der Editor entfernt daraufhin Gruppe und Trenner der Theme-Auswahl aus dem Ansicht-Reiter (`ViewTab.js`), in allen Editoren. Der Mechanismus ist seiner, nur aus der Konfiguration nicht erreichbar: einziger Aufrufer von `setAvailable()` ist eine Windows-XP-Prüfung im Desktop-Programm. |
| `0002-vorgegebenes-theme-gewinnt.patch` | In `htmlutils.js` fällt der Vorbehalt `!window.uitheme.id`. `themeinit.js` läuft vorher und setzt die Kennung aus dem Browserspeicher; ohne diese Änderung bliebe jede früher getroffene Wahl bestehen – und ohne sichtbare Auswahl käme niemand mehr davon los. |

## Ändern

```bash
git clone --depth 1 https://github.com/Euro-Office/web-apps.git /tmp/wa
cd /tmp/wa && git apply /pfad/zu/patches/*.patch
# bearbeiten, dann:
git diff > /pfad/zu/patches/000X-….patch
```
