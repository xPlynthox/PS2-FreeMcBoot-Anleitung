# PS2 SCPH-39004 – Free McBoot auf 8-MB-Memory-Card installieren (Schritt für Schritt)

Diese Anleitung richtet sich ausdrücklich an Personen, die **zum ersten Mal eine PS2 modifizieren**. Jeder benötigte Begriff, Download und Arbeitsschritt wird erklärt.

**Konsole:** PlayStation 2 Fat SCPH-39004 (PAL). **Ziel:** Free McBoot (FMCB) auf einer originalen Sony-8-MB-Memory-Card installieren – ohne DVD-Brenner und ohne bereits modifizierte Karte.

> **Vor dem Beginn:** Das hier ist die **HDD-Startmethode**: Wir bringen die PS2 durch ein kleines Startabbild auf der SSD dazu, einen Dateimanager zu starten. Erst über diesen Dateimanager installieren wir Free McBoot auf die Memory-Card. Die SSD und der USB-Stick sind also zunächst nur Hilfsmittel. **Achtung: Beim Beschreiben der SSD und Formatieren des USB-Sticks werden deren bisherige Daten gelöscht.**
> 

## 1. Was sind diese Programme überhaupt?

| Begriff | Einfach erklärt |
| --- | --- |
| **Free McBoot (FMCB)** | Eine Startsoftware für die PS2-Memory-Card. Damit lassen sich später zusätzliche PS2-Programme starten. |
| **wLaunchELF** | Ein Dateimanager für die PS2, ähnlich dem Windows-Explorer. Er kann z. B. eine Programmdatei vom USB-Stick öffnen. |
| **ELF-Datei** | Eine ausführbare Programmdatei der PS2; vergleichbar mit einer `.exe` unter Windows. |
| **Rufus** | Ein kostenloses **Windows-Programm**, mit dem man USB-Sticks formatieren kann. Es läuft auf dem PC, nicht auf der PS2. |
| **FAT32** | Ein Dateisystem, das die PS2 für USB-Sticks üblicherweise lesen kann. |
| **7-Zip** | Kostenloses Windows-Programm zum Entpacken heruntergeladener `.7z`  • und `.zip`-Archive. |
| **HDD Raw Copy Tool** | Windows-Programm, das eine Abbilddatei (Image) direkt auf eine SSD schreibt. |
| **`__mbr.raw`** | Genau die **wLaunchELF-KHN-HDD-Abbilddatei** für unseren ersten Start. Der Dateiname beginnt mit **zwei Unterstrichen**. |
| **Free HDBoot (FHDB)** | Eine verwandte Startlösung auf der internen Festplatte statt auf der Memory-Card. Für unser erstes Ziel installieren wir **FMCB auf die Karte**. |
| **OPL** | Open PS2 Loader – ein später benötigtes Programm zum Starten von Spielen, z. B. von der internen SSD. |

## 2. Hardware vor dem Start prüfen

- [ ]  PS2 Fat **SCPH-39004** und funktionierender Controller
- [ ]  Originale Sony-**8-MB-Memory-Card** (möglichst mit freiem Speicherplatz)
- [ ]  USB-Stick (sein Inhalt darf gelöscht werden)
- [ ]  SATA-SSD (ihr Inhalt darf gelöscht werden)
- [ ]  Zur PS2 passender **SATA-Netzwerkadapter** für den hinteren Erweiterungsschacht
- [ ]  Windows-PC
- [ ]  Eine Möglichkeit, die SSD mit dem PC zu verbinden: freier SATA-Anschluss **oder** USB-zu-SATA-Adapter/Dock
- [ ]  Die PS2 ist an einem Bildschirm angeschlossen, damit du ihre Menüs sehen kannst

**Wichtig:** Der PS2-SATA-Netzwerkadapter ist zum Einbau in die PS2. Ein zusätzlicher **USB-zu-SATA-Adapter für den PC** ist nur nötig, wenn du die SSD nicht anders am PC anschließen kannst.

## 3. Alle Downloads – und wo genau du sie bekommst

Öffne die folgenden Links auf dem **Windows-PC**. Lade sie möglichst nur von den verlinkten Projekt- und Herstellerseiten.

| Datei / Programm | Wo herunterladen? | Wofür? |
| --- | --- | --- |
| **Rufus** | [rufus.ie/de/](https://rufus.ie/de/) | USB-Stick auf FAT32 formatieren |
| **7-Zip** | [7-zip.org](https://www.7-zip.org/) | Archive entpacken |
| **Free-McBoot-Installer** | [GitHub: Releases](https://github.com/israpps/FreeMcBoot-Installer/releases) | Free McBoot auf der Karte installieren; 1.966 ist eine geeignete Version |
| **wLaunchELF `BOOT.ELF`** | [Direktdownload auf GitHub](https://github.com/israpps/wLaunchELF_ISR/releases/download/latest/BOOT.ELF) | PS2-Dateimanager vom USB-Stick |
| **HDD Raw Copy Tool** | [HDDGURU](https://hddguru.com/software/HDD-Raw-Copy-Tool/) | Startabbild auf die SSD schreiben |
| **`__mbr.raw`** | [**Direktdownload der Datei aus dem GitHub-Projekt**](https://github.com/israpps/FreeMcBoot-Installer/raw/gh-pages/__mbr.raw) | SSD beim ersten Mal zum Starten von wLaunchELF vorbereiten |

### Woher weiß ich, ob ich `__mbr.raw` brauche?

**Ja, bei dieser konkreten Methode brauchst du sie:** Du hast noch **keine FMCB-Memory-Card**, keinen DVD-Brenner und willst über die **interne SSD** zum ersten Mal den PS2-Dateimanager starten. Die genannte Image-Datei ist genau für diesen Einstieg vorgesehen.

**Nein, du brauchst sie nicht**, wenn deine PS2 bereits auf andere Weise wLaunchELF starten kann (z. B. über eine vorhandene FMCB-Karte). Auch nach der erfolgreichen FMCB-Installation wird sie nicht mehr zum **Booten von der Memory-Card** benötigt.

Die Original-Anleitung nennt sie ausdrücklich **„wLaunchELF KHN HDD image“**: [Original-Tutorial](https://israpps.github.io/FreeMcBoot-Installer/test/FHDB-TUTO.html). Dort im Bereich **Requirements → Software → Download into your PC** nach „wLaunchELF KHN HDD image“ und **Download** suchen. Das ist ein alternativer Weg zum selben Download.

**Wenn der direkte Download nicht startet:** Rechtsklick auf den Link → „Link speichern unter“ (sofern vom Browser unterstützt), alternativ den Downloadlink im Original-Tutorial öffnen. Die Datei muss anschließend tatsächlich **`__mbr.raw`** heißen. Windows blendet bekannte Dateiendungen möglicherweise aus: Im Explorer unter **Ansicht → Anzeigen → Dateinamenerweiterungen** einschalten. **Kein beliebiges `mbr.raw` aus einer fremden Quelle** verwenden.

### Dateien am PC übersichtlich ablegen

1. Auf dem Windows-Desktop einen Ordner **`PS2-Modding`** anlegen: Rechtsklick auf freie Stelle → **Neu → Ordner**.
2. Darin die Unterordner **`Downloads`** und **`Entpackt`** anlegen.
3. Die heruntergeladenen Dateien zunächst in **`Downloads`** sammeln.
4. 7-Zip bei Bedarf installieren.
5. Auf das heruntergeladene Free-McBoot-Archiv rechtsklicken → **7-Zip → Entpacken nach …**; den entpackten Ordner nach **`Entpackt`** legen.
6. Die Datei **`__mbr.raw`** nicht mit Rufus formatieren und nicht einfach auf den USB-Stick kopieren. Sie bleibt zunächst auf dem PC.

## 4. USB-Stick vorbereiten – Rufus genau erklärt

**Was macht Rufus?** Es richtet auf deinem **PC** den USB-Stick mit dem Dateisystem FAT32 ein, damit der PS2-Dateimanager die Dateien darauf lesen kann. Rufus wird **nicht** auf der PS2 installiert.

**Achtung: Alle vorhandenen Dateien auf dem ausgewählten USB-Stick werden gelöscht.**

1. Den USB-Stick in den Windows-PC stecken.
2. [Rufus](https://rufus.ie/de/) herunterladen (z. B. die portable Windows-Version) und starten.
3. Eine Windows-Sicherheitsabfrage gegebenenfalls bestätigen.
4. Oben unter **Laufwerk** den USB-Stick anhand seiner **Größe** auswählen. Nicht versehentlich die SSD oder eine andere Festplatte wählen.
5. Unter **Startart**: **Nicht startfähig**.
6. Unter **Partitionsschema**: **MBR**, sofern diese Auswahl angeboten wird.
7. Unter **Dateisystem**: **FAT32**.
8. **Clustergröße** unverändert auf Standard lassen.
9. Unter **Laufwerksbezeichnung** optional **`PS2USB`** eingeben.
10. Auf **START** klicken, Löschwarnung bestätigen und warten, bis der Vorgang abgeschlossen ist.
11. Rufus schließen und im Windows-Explorer prüfen, dass sich der USB-Stick öffnen lässt.

**Wenn FAT32 nicht angeboten wird:** Nicht ohne Prüfung NTFS wählen. Gegebenenfalls einen kleineren USB-Stick verwenden oder ein geeignetes FAT32-Formatierungswerkzeug einsetzen.

## 5. Dateien auf den USB-Stick kopieren

1. Den **entpackten** Free-McBoot-Installer-Ordner im Windows-Explorer öffnen.
2. Den Installer-Ordner **vollständig** auf den USB-Stick kopieren: mit allen Unterordnern und Dateien. Nichts aus **`INSTALL`** weglassen.
3. Die heruntergeladene Datei **`BOOT.ELF`** direkt ins **Hauptverzeichnis** des USB-Sticks kopieren (also nicht in einen anderen Ordner).
4. Prüfen, dass im Installer-Ordner die eigentliche **`FMCBInstaller.elf`** liegt. Groß-/Kleinschreibung und Ordnername können je nach Release anders aussehen.

Beispiel (der Installer-Ordner kann tatsächlich anders heißen):

```
USB-Stick (PS2USB)
├── BOOT.ELF
└── FMCBinst-1966/
    ├── FMCBInstaller.elf
    ├── INSTALL/
    └── weitere Dateien/Ordner des Installers
```

**Wichtig:** Das USB-Laufwerk wird unter Windows als Buchstabe (z. B. `E:\`) angezeigt; auf der PS2 heißt es später **`mass:/`**. Das ist **derselbe USB-Stick**.

## 6. SSD am Windows-PC vorbereiten

**Was passiert?** Die Datei **`__mbr.raw`** wird mit dem Windows-Programm **HDD Raw Copy Tool** auf die SSD geschrieben. Sie ist kein normales Dokument zum Hineinkopieren, sondern ein Startabbild, das an den Anfang des Laufwerks geschrieben werden muss.

**Ganz wichtig:** Dabei können vorhandene Partitionen und Daten der SSD verloren gehen. Die SSD muss für diesen Vorgang entbehrlich sein.

1. **PS2 ausschalten** und die SSD zunächst **am Windows-PC** anschließen: über SATA oder über den USB-zu-SATA-Adapter.
2. Im Windows-Explorer bzw. der Datenträgerverwaltung die **Größe und Modellbezeichnung** dieser SSD notieren. Das hilft, sie gleich eindeutig zu erkennen.
3. [HDD Raw Copy Tool](https://hddguru.com/software/HDD-Raw-Copy-Tool/) herunterladen und starten; bei Bedarf Rechtsklick → **Als Administrator ausführen**.
4. In der **Quellenauswahl (SOURCE)** auf **FILE** gehen und die heruntergeladene **`__mbr.raw`** auswählen. Die Datei kommt vom GitHub-Link in Abschnitt 3.
5. Auf **Continue** klicken.
6. Bei der **Zielauswahl (TARGET)** **ausschließlich die SSD für die PS2** auswählen. **Unbedingt Modell und Größe kontrollieren!** Niemals die Windows-Systemplatte auswählen.
7. Auf **Continue** klicken und vor dem Schreiben Quelle und Ziel erneut kontrollieren.
8. Mit **START** das Schreiben starten und Warnungen zum Überschreiben nur bestätigen, wenn die richtige SSD ausgewählt ist.
9. Auf die Meldung warten, dass das Schreiben abgeschlossen ist.
10. Wenn Windows die SSD danach **initialisieren oder formatieren** will: **Abbrechen**. Die PS2 verwendet hierfür eine eigene Datenstruktur.
11. SSD sicher vom PC trennen.

**Hinweis:** `__mbr.raw` ist das vom verlinkten Projekt vorgesehene **kleine HDD-Bootstrap-Image**, kein mehrere Hundert Gigabyte großes Komplettabbild einer Spiele-SSD. Nach diesem Schritt ist die SSD **noch nicht** für das Speichern deiner Spiele eingerichtet.

## 7. Alles an der PS2 anschließen und erstmals starten

1. Die PS2 ausschalten; hinten den Netzschalter auf **0** stellen.
2. Die SSD mit dem SATA-Anschluss des PS2-Netzwerkadapters verbinden.
3. Den Netzwerkadapter mit SSD vorsichtig in den **hinteren Erweiterungsschacht** der PS2 einsetzen und befestigen; nichts mit Gewalt drücken.
4. Die originale Sony-8-MB-Memory-Card in **Memory-Card-Slot 1** (vorn an der PS2) stecken.
5. Den vorbereiteten USB-Stick in einen der **vorderen USB-Anschlüsse** der PS2 stecken.
6. Controller anschließen, Bildschirm einschalten, PS2-Netzschalter hinten auf **I** stellen.
7. PS2 mit der **Power-Taste** starten.
8. Prüfen, ob der **wLaunchELF-KHN-Dateimanager** erscheint. Laut Original-Tutorial kann der erste Start etwas dauern.

**Wenn nur das normale Sony-Menü erscheint:** Nicht einfach weiterklicken oder die SSD neu formatieren. Bei manchen PS2-Geräten ist das automatische Starten von HDD-Software in der Konfiguration deaktiviert. Dann funktioniert diese Einstiegsmethode nicht und ein alternativer Einstieg (etwa ein kompatibler FreeDVDBoot-Weg oder eine vorhandene FMCB-Karte) ist erforderlich. Das bedeutet nicht automatisch, dass der USB-Stick falsch vorbereitet ist.

## 8. Vom ersten wLaunchELF zum vollständigen PS2-Dateimanager

**Warum zwei Programme?** Das SSD-Abbild startet einen kleinen wLaunchELF-KHN-Dateimanager. Von dort starten wir die neuere **`BOOT.ELF`**, die wir auf den USB-Stick kopiert haben.

1. Im angezeigten wLaunchELF-KHN-Menü **FileBrowser** auswählen.
2. Mit der auf dem Bildschirm angegebenen Bestätigungstaste öffnen (oft **Kreis**; die Tastenbelegung kann variieren).
3. **`mass:/`** auswählen: Das bedeutet **USB-Stick**.
4. Dort **`BOOT.ELF`** markieren und starten.
5. Das wLaunchELF-Menü erscheint erneut, möglicherweise mit einer **anderen Hintergrundfarbe**. Jetzt läuft das zusätzliche Programm vom USB-Stick.

**Zwischenschritt für eine Spiele-SSD (noch nicht für FMCB auf der Karte notwendig):** In der ursprünglichen Free-HDBoot-Anleitung folgt jetzt **`MISC:/ → HDDManager → R1 → Format`**, wodurch die SSD für das klassische PS2-HDD-Dateisystem eingerichtet wird. **Dieser Schritt löscht die SSD** und dient der späteren HDD-/FHDB-Einrichtung. **Nicht mit der Formatierung der 8-MB-Memory-Card verwechseln.** Da unser unmittelbares Ziel nur FMCB auf der Memory-Card ist, lassen wir den optionalen HDD-Formatierungsschritt zunächst aus.

## 9. Free McBoot auf die originale 8-MB-Memory-Card installieren

1. Im neu gestarteten wLaunchELF **FileBrowser** öffnen.
2. Den Eintrag **`mass:/`** wählen (dein USB-Stick).
3. Den Ordner des entpackten Free-McBoot-Installers öffnen.
4. **`FMCBInstaller.elf`** auswählen und ausführen.
5. Im Installer **Install** wählen (nicht **Install FHDB** – das wäre die Installation auf der Festplatte).
6. Falls die Installation nach einer Variante fragt, die **normale Installation für diese Konsole** wählen.
7. Als Ziel **Memory Card Slot 1 / `mc0:`** auswählen. **`mc0:`** bedeutet Memory-Card-Slot 1; **`mc1:`** wäre Slot 2.
8. Die Bestätigungsabfragen auf dem Bildschirm lesen und die Installation starten.
9. Warten, bis eine Erfolgsmeldung erscheint; die PS2 währenddessen **nicht ausschalten**.
10. Installer beenden.

**Wenn auf der Karte Spielstände sind:** Vorher möglichst sichern. Nicht unüberlegt die Memory-Card formatieren. **Multi-Install** ist für diese eine SCPH-39004 nicht notwendig und wird beim hier verlinkten Installer sogar aus Sicherheitsgründen unterbunden.

## 10. Kontrolle: Hat die Memory-Card funktioniert?

1. PS2 vollständig ausschalten.
2. USB-Stick abziehen, die originale 8-MB-Memory-Card aber in **Slot 1** lassen.
3. PS2 starten und prüfen, ob zusätzliche FMCB-Menüpunkte wie **Free McBoot Configurator** oder **LaunchELF** erscheinen.
4. **Eindeutiger Test:** PS2 wieder ausschalten, hinten auf **0** stellen, SSD-Netzwerkadapter entfernen. Memory-Card eingesetzt lassen.
5. PS2 ohne SSD erneut einschalten. Wenn das FMCB-Menü erscheint, startet die Karte unabhängig vom USB-Stick und unabhängig von der SSD.

## 11. Erst danach: SSD für OPL und Spiele einrichten

- **FMCB auf der Memory-Card** ist das erste, getrennte Ziel.
- Die mit **`__mbr.raw`** vorbereitete SSD ist anschließend noch **keine fertige Spiele-SSD**.
- Für Spiele muss die SSD später entsprechend der gewählten **OPL-/HDD-Methode** eingerichtet werden. Bei der klassischen Methode gehören dazu HDD-Formatierung, Partitionierung und die Installation von OPL; das wird am besten in einer **eigenen Anleitung** durchgeführt.
- **Wichtig:** Den gerade geschriebenen HDD-Bootstrap nicht mit einer fertigen Free-HDBoot-Installation verwechseln.

## 12. Fehlerbehebung für Anfänger

| Problem | Zuerst prüfen |
| --- | --- |
| **Wo ist `__mbr.raw`?** | Den Direktdownload aus Abschnitt 3 verwenden; zwei Unterstriche vor `mbr.raw`. |
| **Brauche ich `__mbr.raw`?** | Ja, für genau diesen Erststart über die SSD ohne FMCB-Karte; nein, wenn bereits ein anderer wLaunchELF-Startweg vorhanden ist. |
| **Was ist Rufus?** | Ein Windows-Programm zum Formatieren des USB-Sticks – siehe Abschnitt 4. |
| **USB-Stick auf der PS2 nicht sichtbar** | FAT32 prüfen, anderen vorderen USB-Port testen, Stick gegebenenfalls wechseln. |
| **Ich sehe kein `BOOT.ELF`** | Liegt die Datei wirklich im Hauptverzeichnis des USB-Sticks? |
| **Installer-Datei nicht gefunden** | Entpackten Installer-Ordner samt Unterordnern prüfen; nicht bloß das `.7z`-Archiv kopieren. |
| **PS2 zeigt nur Sony-Browser** | HDD-Start möglicherweise deaktiviert oder SSD/Adapter/Image nicht passend; keinen weiteren Formatversuch auf Verdacht. |
| **FMCB startet ohne SSD nicht** | Slot 1 prüfen, Erfolgsmeldung der Installation und korrekte Installationsvariante kontrollieren. |
