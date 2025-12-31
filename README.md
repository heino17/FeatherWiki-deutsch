# Feather Wiki Deutsch
Die Übersetzung besteht/bestand darin eine de.yml im Ordner /rails/config/locales/ zu erzeugen und in's Deutsche zu übersetzen.
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

Feather Wiki uses only a few JavaScript dependencies to function on the front end, but it requires more to develop.

To get your computer set up to develop:

1. Install [Git](https://git-scm.com)
1. Install [Node](https://nodejs.org)
1. Use a command line or terminal
1. Clone the git repo with `git clone https://codeberg.org/Alamantus/FeatherWiki.git`
1. Navigate to your cloned repo `cd FeatherWiki`
1. Run `npm install`
1. Run `npm start` and visit http://localhost:3000 in your browser
1. Start making changes to the JavaScript to update your build—you will need to refresh your browser to see your changes
  - Note: Changing the CSS doesn't automatically update the build, so you'll need to modify some JS or restart the script to see those changes

When you're ready to build, simply use the `npm run build` to build Feather Wiki.

To test a build, you can use `npm test` to build and serve the Server build on a local http server. The test script will allow
Feather Wiki to use the "Save Wiki to Server" button—the output gets saved to `develop/put-save.html` if you need
to check it.

### Details

Feather Wiki uses a modified version of [Choo](https://choo.io) as its [base JavaScript framework](./nanochoo.js), a subset of [JSON-Compress](https://github.com/Alamantus/JSON-Compress) for minifying JSON output, a customized [pell](https://jaredreich.com/pell/) for its [HTML editor](./helpers/ed.js), and a greatly customized [md.js](https://github.com/thysultan/md.js) for its [Markdown parsing](./helpers/md.js).

The overarching goal is to keep Feather Wiki as small as possible while still providing the most important features. Unfortunately, that's a pretty loose and fluid goal, but as long as you keep "as small as possible" in mind, you probably won't go too far astray.

## License Clarification

Feather Wiki is an HTML document containing a self-replicating JavaScript application for creating wiki-style websites whose content is also stored in the output.  
Copyright (C) 2022 [Robbie Antenesse](https://robbie.antenesse.net) \<dev@alamantus.com\>

Feather Wiki is free software: you can redistribute it and/or modify
it under the terms of the GNU Affero General Public License as
published by the Free Software Foundation, either version 3 of the
License, or (at your option) any later version.

Any content created by a user using any version of Feather Wiki is
the property of its creator. User-created data and the replicated
copies of Feather Wiki containing user-created data can be used and
distributed however their creator see fit. The GNU Affero General
Public License applies to the code that constitutes the Feather Wiki
application and NOT the content created by users of Feather Wiki
unless explicitly stated by the user within their own content.

Feather Wiki is distributed in the hope that it will be useful,
but WITHOUT ANY WARRANTY; without even the implied warranty of
MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE. See the
GNU Affero General Public License for more details.

A [copy of the GNU Affero General Public License](https://codeberg.org/Alamantus/FeatherWiki/src/branch/main/LICENSE)
is included in the source code of Feather Wiki. If not, see
https://www.gnu.org/licenses/.
