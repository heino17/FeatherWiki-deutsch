# Feather Wiki Deutsch
Die Übersetzung besteht/bestand darin, eine de-DE.json im Ordner /locales zu erzeugen und in's Deutsche zu übersetzen.
Für den schnellen Überblick hier die original README in deutscher Sprache. 


## Feather Wiki

Ein 58,163 Kilobyte kleines [quine](https://en.wikipedia.org/wiki/Quine_(computing)) zum Erstellen einfacher, in sich geschlossener Wikis. Die Idee dahinter ist, dass es wie [TiddlyWiki](https://tiddlywiki.com) funktioniert, aber so klein wie möglich ist.

Schau Dir die [Dokumentation](https://feather.wiki) an, um zu verstehen, wie es funktioniert und zu lernen, wie man es verwendet!

## Browser-Kompatibilität

Feather Wiki läuft nur auf Browsern, die die Funktionen von [ECMAScript 2015](https://caniuse.com/es6) (auch bekannt als ES6) unterstützen.

<details>
<summary>👨‍💻 Technischer Hinweis: Unterstützte Browser</summary>

Laut [dieser ECMAScript-Kompatibilitätstabelle](https://compat-table.github.io/compat-table/es6/) sollten die folgenden Browserversionen Feather Wiki Version 1.3.0 und höher definitiv ohne Probleme ausführen können:

- Chrome 86+
- Edge 87+
- Firefox 88+
- iOS Safari 12+
- Opera 73+
- Opera Mobile 62+
- Safari 13+
- Samsung Internet for Android 12+

Die oben verlinkte Tabelle ist unvollständig. Wenn Dein Browser älter als einer der aufgeführten ist, kannst Du Feather Wiki _möglicherweise_ dennoch ausführen, müsst jedoch selbst überprüfen, ob er die [Funktionen von ECMAScript 2015](https://caniuse.com/es6) unterstützt.

</details>

### Server-Speichern

Feather Wiki enthält Code zum Speichern auf Webservern, die auf eine bestimmte Weise eingerichtet sind. Es erwartet einen `dav`-Header mit einem beliebigen Wert, der von einem `OPTIONS`-Aufruf an den Server unter derselben Adresse zurückgegeben wird, unter der die Feather Wiki-Datei bereitgestellt wird. Wenn der Server kompatibel erscheint, zeigt Feather Wiki über der Schaltfläche "Wiki lokal speichern" die Schaltfläche "Wiki auf Server speichern" an.

Wenn der Benutzer auf die Schaltfläche "Wiki auf Server speichern" klickt, sendet Feather Wiki eine `PUT`-Anfrage an den Server mit einem Textkörper, der die vollständige HTML-Ausgabe der Feather Wiki-Datei enthält, die normalerweise auf den Computer heruntergeladen würde. Wenn Du Dein Wiki mit einem Passwortschutz versehen möchtest (und ich denke, das _solltest_ Du auch), musst Du dies auf eine Weise umsetzen, die der Server verstehen kann, sei es, indem sich der Benutzer auf einer anderen Seite anmeldet und in einem Domain-Cookie gespeichert wird, oder indem Du die grundlegende HTTP-Authentifizierung verwendest – die Wahl liegt bei Dir.

Nach dem Senden an den Server erwartet Feather Wiki entweder eine erfolgreiche oder eine fehlgeschlagene Antwort mit einer optionalen Textnachricht als Hauptteil, um einen Fehler zu erklären. Wenn bei einem Fehler kein Text im Hauptteil der Antwort zurückgegeben wird, wird lediglich der Statuscode in einem Meldungsfeld angezeigt, z. B. "Speichern fehlgeschlagen! Status 403". Bei Erfolg zeigt Feather Wiki "Gespeichert" an.

Du kannst diese Funktion auf [Tiddlyhost](https://tiddlyhost.com) oder mithilfe eines [selbst gehosteten Nestes](https://codeberg.org/Alamantus/FeatherWiki/src/branch/main/nests) aus diesem Repository ausprobieren!

## Federnkleid & Grundgerüst

Die CSS- und JavaScript-Dateien von Feather Wiki sind separat vom vollständigen HTML verfügbar, aber es ist wichtig zu beachten, dass JavaScript derzeit erwartet, dass sowohl sein Code als auch das CSS _auf der HTML-Seite_ vorhanden sind, die vollständig geladen ist, um gespeichert zu werden! Insbesondere muss der Inhalt der `.js`-Datei in der HTML-Ausgabe innerhalb eines `<script id="a">`-Skript-Tags mit der angegebenen ID (`a`) stehen, und der Inhalt der `.css`-Datei muss in der HTML-Ausgabe innerhalb eines `<script id="s">`-Style-Tags mit der angegebenen ID (`s`) stehen. Wenn Du Dein Wiki nicht speichern musst, musst Du dies nicht tun.

## Beitragen

Weitere Informationen darüber, wie Du bei diesem Projekt mithelfen kannst, findest Du in der Datei [CONTRIBUTING.md](CONTRIBUTING.md). Dort findest Du auch Details zum Hinzufügen von nicht-englischen Übersetzungen zum Feather Wiki.

Wenn Du den Entwickler finanziell unterstützen möchtest, kannst Du einmalige Spenden über <https://buymeacoffee.com/robbieantenesse> oder <https://ko-fi.com/robbieantenesse> senden oder über <https://liberapay.com/robbieantenesse> wiederkehrende Spenden einrichten. Dies ist absolut nicht erforderlich, wird aber sehr geschätzt!

## Entwicklung

Feather Wiki benötigt nur wenige JavaScript-Abhängigkeiten, um im Frontend zu funktionieren, aber für die Entwicklung sind mehr erforderlich.

So richtest DU Deinen Computer für die Entwicklung ein:

1. [Git](https://git-scm.com) installieren
1. [Node](https://nodejs.org) installieren
1. Eine Kommandozeile oder ein Terminal verwenden
1. Klonen des Git-Repositorys mit `git clone https://codeberg.org/Alamantus/FeatherWiki.git`
1. Navigiere zu Deinem geklonten Repo `cd FeatherWiki`
1. `npm install` ausführen
1. `npm start` ausführen und http://localhost:3000 in Deinem Browser aufrufen
1. Beginne mit den Änderungen am JavaScript, um Ihren Build zu aktualisieren – Du musst Deinen Browser aktualisieren, um die Änderungen zu sehen.
  - Hinweis: Durch Ändern des CSS wird der Build nicht automatisch aktualisiert. Du musst also etwas JS ändern oder das Skript neu starten, um die Änderungen zu sehen.

Wenn Du bereit bist zu bauen, verwende einfach `npm run build`, um Feather Wiki zu erstellen.

To test a build, you can use `npm test` to build and serve the Server build on a local http server. The test script will allow
Feather Wiki to use the "Save Wiki to Server" button—the output gets saved to `develop/put-save.html` if you need
to check it.

### Einzelheiten

Feather Wiki verwendet eine modifizierte Version von [Choo](https://choo.io) als [Basis-JavaScript-Framework](./nanochoo.js), eine Untergruppe von [JSON-Compress](https://github.com/Alamantus/JSON-Compress) zur Minimierung der JSON-Ausgabe, ein angepasstes [pell](https://jaredreich.com/pell/) für seinen [HTML-Editor](./helpers/ed.js) und ein stark angepasstes [md.js](https://github.com/thysultan/md.js) für seine [Markdown-Parsing](./helpers/md.js).

Das übergeordnete Ziel ist es, Feather Wiki so klein wie möglich zu halten und gleichzeitig die wichtigsten Funktionen bereitzustellen. Leider ist das ein ziemlich vages und schwammiges Ziel, aber solange Sie "so klein wie möglich" im Hinterkopf behalten, werden Sie wahrscheinlich nicht allzu weit daneben liegen.

## Lizenzklärung
Feather Wiki ist ein HTML-Dokument, das eine sich selbst replizierende JavaScript-Anwendung zum Erstellen von Websites im Wiki-Stil enthält, deren Inhalt ebenfalls in der Ausgabe gespeichert wird.  
Copyright (C) 2022 [Robbie Antenesse](https://robbie.antenesse.net) \<dev@alamantus.com\> 

Feather Wiki ist freie Software: Sie können sie unter den Bedingungen der GNU Affero General Public License, 
wie von der Free Software Foundation veröffentlicht, weitergeben und/oder modifizieren, 
entweder gemäß Version 3 der Lizenz oder (nach Ihrer Wahl) jeder späteren Version.

Alle Inhalte, die von einem Benutzer mit einer beliebigen Version von Feather Wiki erstellt wurden, sind
Eigentum ihres Erstellers. Vom Benutzer erstellte Daten und die replizierten
Kopien von Feather Wiki, die vom Benutzer erstellte Daten enthalten, können verwendet und
verbreitet werden, wie es ihr Ersteller für richtig hält. Die GNU Affero General
Public License gilt für den Code, aus dem die Feather Wiki-Anwendung besteht,
NICHT jedoch für die von Benutzern von Feather Wiki erstellten Inhalte,
es sei denn, dies wird vom Benutzer in seinen eigenen Inhalten ausdrücklich angegeben.

Feather Wiki wird in der Hoffnung verteilt, dass es nützlich ist,
jedoch OHNE JEGLICHE GARANTIE, auch ohne die implizite Garantie der
MARKTGÄNGIGKEIT oder EIGNUNG FÜR EINEN BESTIMMTEN ZWECK. Weitere Informationen finden Sie in der
GNU Affero General Public License.

Eine [Kopie der GNU Affero General Public License](https://codeberg.org/Alamantus/FeatherWiki/src/branch/main/LICENSE)
ist im Quellcode von Feather Wiki enthalten. Falls nicht, siehe
https://www.gnu.org/licenses/.
