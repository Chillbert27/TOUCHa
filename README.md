TOUCHa 1.5.2-beta — Host-Set für Linux
=======================================

Dieses Set enthält den HOST-Teil: Streamer, Steuer-GUI und Installer.
Es enthält bewusst KEINE Viewer-APK — die Viewer-Apps für Quest und für
Tablet/Handy werden getrennt verteilt, sie unterliegen anderen
Lizenzbedingungen als der Host.

Inhalt
------
  install.sh         Installer (nativ, oder Flatpak)
  toucha-streamer    der Streamer, Release-Build, gestripped
  toucha_gui.py      Steuer-GUI
  lib/               mitgelieferte Laufzeitbibliotheken (siehe unten)
  toucha-release.gpg öffentlicher Release-Schlüssel
  SHA256SUMS.txt     Prüfsummen
  SHA256SUMS.asc     GPG-signiert
  LICENSE NOTICE SOURCE-OFFER.txt licenses/

Signatur
--------
GPG-Key: TOUCHa Releases (TOUCHa release signing)
         DB7A3F896919DA51825F3C59715A9113AF69D487

Der Key ist nicht im lokalen Schlüsselbund als vertrauenswürdig
markiert. Das ist erwartbar: der Installer vergleicht den Fingerabdruck
selbst mit dem oben genannten. Eine gelbe "nicht vertrauenswürdig"-
Warnung von gpg ist in Ordnung, ein ABBRUCH des Installers nicht.


Die mitgelieferten Bibliotheken
==============================
lib/ enthält neun Bibliotheken, die der Streamer zur Laufzeit braucht:

  libavcodec.so.61      libavutil.so.59     libswscale.so.8
  libswresample.so.5    libx264.so.165      libsdbus-c++.so.1
  libpipewire-0.3.so.0  libei.so.1          libxkbcommon.so.0

Bewusst NICHT dabei: Opus und AV1 (in dieser Version entfernt — Ton ist
reines PCM, Video HEVC/H.264). libopus.so.0 bleibt ein System-Rest, weil
die Binary den alten Loader-Eintrag noch trägt; der Installer erkennt
das und installiert ihn bei Bedarf aus den Distro-Paketen nach.

Die Binary ist mit RUNPATH=$ORIGIN/lib gelinkt, findet ihre Bibliotheken
also immer neben sich — unabhängig davon, was auf dem Zielrechner
installiert ist. Das ist der Grund, warum sie hier liegen: der Streamer
verlangt exakt diese Versionen, und auf einem anderen Rechner sind sie
sonst weder vorhanden noch die richtigen.

Ohne dieses Verzeichnis startet der Streamer auf den meisten Rechnern
nicht ("libavcodec.so.61 not found").


Installation
============
  tar xzf toucha-1.5.2-host-linux.tar.gz
  cd toucha-1.5.2-host-linux
  ./install.sh --verify-only     nur prüfen, nichts installieren
  ./install.sh                   installieren (fragt bei fehlenden
                                 Systempaketen nach)
  ./install.sh --yes             installieren ohne Rückfrage (Skripte)
  ./install.sh --no-sysdeps      nie den Paketmanager anfassen, nur melden
  ./install.sh --launch          installieren und GUI starten
  ./install.sh --flatpak         Flatpak statt nativer Installation

Fehlende Systempakete (libopus, PipeWire-Daemon, PyQt6) installiert der
Installer nur mit ausdrücklicher Zustimmung (--yes oder [j/N]-Antwort).
Ohne Zustimmung zeigt er die exakten Befehle und bricht ab — es wird
nie still ein sudo ausgeführt.

Bei einem Update zuerst:
  ./install.sh --uninstall


Systemvoraussetzungen
=====================
  * PipeWire mit laufendem Session-Bus (für Bildschirmaufnahme)
  * libopus (System-Rest, s. o. — installiert der Installer bei Bedarf)
  * Python 3 mit PyQt6 (nur für die GUI)
  * OpenSSL, X11 (Basis-System)
  * x86_64, Linux

Fehlt der PipeWire-Daemon, bricht der Streamer mit einer Meldung ab
(frisch installiertes PipeWire braucht ggf. ein Re-Login).


Direkt starten
==============
  ./toucha-streamer --source portal --monitors 3 \
      --codec hevc --fps 30 --port 8778 --bitrate 7500 \
      --audio system --audio-codec pcm --restore

Das Portal fragt einmal nach der Bildschirmfreigabe und merkt sich das
Token — danach erscheint kein Dialog mehr. Ohne --restore kommt der
Dialog bei jedem Start.

Nützliche Zusatzflags (alle mit --help ausführlich beschrieben):

  --monitor-mode physical   Monitore direkt statt als Fenster
  --restore                 Portal-Freigabe-Token wiederverwenden
  --smooth off              Touch-Glättung aus
  --no-audio-takeover       Tonausgabe des Hosts unangetastet lassen
  --no-mic-return           kein Viewer-Mikrofon an den Host
  --clipboard-sync          Host schickt die Zwischenablage mit (der
                            Viewer fragt ohnehin nach)


Was in dieser Version geändert wurde
===================================
Bilder
  * Der Connect-Screen bekommt wieder sein Bild. Zwei Ursachen: der Host
    stellte START erst NACH dem Screenshot-Vorgang zu, und das Wecken der
    Aufnahme dauert bis zu 2,5 s — der Viewer wartete in dieser Zeit auf
    START, lief in sein Socket-Timeout und hat die Sitzung weggeworfen. START
    geht jetzt sofort raus, das Bild wird danach angefordert.
  * Eine Bildanfrage geht nicht mehr verloren, wenn der Versand einmal nicht
    klappt: der Connect-Screen fragt genau einmal, ein verworfener Wunsch
    blieb fuer immer eine leere Karte.

Mehrere Viewer
  * Mehrere Geräte sehen denselben Monitor gleichzeitig. Bild und Ton
    bleiben EIN Strom mit EINEM SRTP-Schlüssel je Kopplung — nur die
    Zieladresse und die Rechte unterscheiden sich pro Gerät.
  * Geht ein Gerät weg, verlieren die anderen nichts; erst der letzte
    Abgang beendet die Sitzung. Ein Gerät, das den Ton abschaltet, schaltet
    ihn nicht für die anderen ab.
  * Wie viele Geräte gleichzeitig sehen dürfen, ergibt sich aus den
    Gekoppelten (Leserechte eines Geräts = ein gleichzeitiger Platz) und
    wird bei jedem Verbinden neu gelesen: ein neu gekoppeltes Gerät ist
    sofort erlaubt, ohne Neustart. Nicht gekoppelte Geräte sehen nichts.
  * Ein LIMIT (kein Platz frei) geht nur an das betroffene Gerät, nicht an
    die anderen Sitzungen.
  * Thumbnails (Connect-Screen) gehen nur an das Gerät, das sie angefordert
    hat, statt an alle.
  * Verlässt ein Gerät die Sitzung mitten im Schreiben, bringt das den Host
    nicht mehr um (SIGPIPE) — vorher riss es allen anderen das Bild weg.

Zwischenablage
  * Host → Viewer nur mit ausdrücklicher Zustimmung: der Viewer fragt, und
    der Host-Push wird mit --clipboard-sync freigeschaltet. Ohne beides
    fließt nichts.
  * Viewer → Host nur nach Bestätigung im Viewer.
  * Das Recht "keine Zwischenablage" gilt auch bei mehreren Geräten, und
    es gilt pro Gerät: ein Gerät ohne Recht entscheidet nicht über die
    anderen.

Monitore
  * Die Monitorzahl aus Discovery setzt die Obergrenze wieder richtig; ein
    Intent-Hinweis oder ein alter Cache-Eintrag senkt sie nicht mehr.
  * Der Connect-Screen fragt pro Monitor eine Karte ab, und jede bekommt
    ihr eigenes Bild.

Bild
  * Thumbnails im Connect-Screen funktionieren wieder. Vorher wurde die
    Monitor-Nummer doppelt auf den Port gerechnet: Monitor 1 bekam das
    Bild von Monitor 2, Monitor 2 fragte einen Port ab, auf dem niemand
    hört — die Karten blieben leer.
  * Absturz auf Android 5 behoben.

Ton
  * Das Ruckeln ist weg. Ursache: der Streamer las die Audioquelle,
    bevor er den 20-ms-Takt hielt. Dadurch entstand Rückstand, der als
    Burst abgeschickt wurde und den Puffer im Viewer flutete.
  * Latenz gesenkt: Audio-Puffer von rund 300 ms auf 160 ms.
  * Das Audio-Routing war kaputt: der Streamer setzte sich selbst als
    Standard-Tonausgabe und stellte beim Beenden ein leeres Ausgabegerät
    wieder her, wodurch beide echten Ausgänge stumm blieben.
  * Dauerhaftes Stehlenbleiben behoben.

Codecs
  * Opus und AV1 entfernt. Ton ist reines PCM (48 kHz, Stereo).
  * Der Viewer erwartet HEVC. H.264 nur, wenn der Host keinen
    HEVC-Encoder hat. Kein Umschalten mitten im Bild mehr.

Automatik
  * Der Viewer meldet dem Host, was er empfangen kann; der Host wählt
    Auflösung und Bitrate daraus. Kein Aushandeln während der Sitzung.
  * Die Bitrate sinkt auch, wenn der Viewer seinen Audiopuffer-Überlauf
    meldet — am Packet-Loss-Zähler sieht man das nicht.
  * Bildrate folgt der Bewegung: 1 fps bei statischem Bild, volle Rate
    bei Bewegung. Die Eingabe hängt NICHT daran.

Installer
  * lib/ jetzt mit PipeWire, libei, xkbcommon (9 statt 6 Dateien).
  * Fehlende Systempakete installiert der Installer bei Bedarf nach —
    nur mit Zustimmung (--yes oder [j/N]), nie still.

Bedienung
=========
Im Host-GUI sind zwei Schalter neu, beide standardmäßig AN:
  Audio takeover    darf ein Viewer die Standard-Tonausgabe übernehmen
  Viewer mic return  Mikrofon des Viewers an den Host zurückgeben


Bekannte Punkte
===============
  * Bei völlig statischem Bild aktualisiert sich das Bild nur einmal pro
    Sekunde; eine sichtbare Bewegung kann bis zu eine Sekunde brauchen.
    Die Eingabe ist davon nicht betroffen.
  * PCM-Ton belegt 1,5 Mbit/s. Bei schwachem WLAN nur den Videostream
    verkleinern, sonst wird der Ton abgeschnitten.
