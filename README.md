# laMiaTV

![Screenshot](https://boncoddo.altervista.org/img/github/laMiaTV.png)

**laMiaTV** è un **player IPTV** che consente di leggere una lista canali in formato **_M3U_**, da cui è possibile avviare un determinato canale grazie al lettore video integrato del televisore.
L'app è compatibile con le TV Samsung che hanno il sistema operativo **Orsay o Tizen**, sul sistema **WebOS** di LG e con altri sistemi supportati dal framework **Apache Cordova**.


## Prerequisiti

  **laMiaTV** ha bisogno dei seguenti software per funzionare, si prega di installarli:
  
  1. [Node.js](https://nodejs.org/)
  2. [Git](https://git-scm.com/)
  3. [Browser Chrome](https://www.google.it/chrome/)
  4. [Samsung Tizen SDK](https://developer.tizen.org/development/tizen-studio/download) (solo per Tizen OS)
  5. [LG WebOS SDK](http://webostv.developer.lge.com/sdk/installation/) (solo per WebOS)
  6. Moduli npm (cordova, grunt), per installarli digitare dal prompt dei comandi:
  ```bash
    $ npm install -g cordova
    $ npm install -g grunt-cli
  ```


## Installazione

  1. Creare la cartella principale del progetto
  ```bash
  $ mkdir SmartTV-Dev
  ```
  2. Entrare nella directory appena creata
  ```bash
  $ cd SmartTV-Dev
  ```
  3. Effettuare **git clone** dei repository seguenti:
  ```bash
  $ git clone https://github.com/apache/cordova-js.git
  $ git clone https://github.com/Samsung/cordova-plugin-toast.git
  $ git clone https://github.com/Samsung/cordova-sectv-orsay.git
  $ git clone https://github.com/Samsung/cordova-sectv-tizen.git
  $ git clone https://github.com/Samsung/cordova-tv-webos.git
  $ git clone https://github.com/Samsung/grunt-cordova-sectv.git
  ```
  4. Aprire il file presente in **_cordova-js/Gruntfile.js_** in un editor, ed aggiungere la virgola e le seguenti righe al *compile*:
  ```javascript
  module.exports = function(grunt) {
grunt.initConfig({
    pkg: grunt.file.readJSON('package.json'),
    compile: {
        ...,
        "sectv-orsay": {},
        "sectv-tizen": {},
        "tv-webos": {}
    },
  ```
  5. Allo stesso modo, aggiungere al file presente in **_cordova-js/package.json_** le seguenti righe:
  ```javascript
    ...,
    "cordova-platforms": {
        "cordova-sectv-orsay": "../cordova-sectv-orsay",
        "cordova-sectv-tizen": "../cordova-sectv-tizen",
        "cordova-tv-webos": "../cordova-tv-webos"
    }
}
  ```
  6. Infine, sempre nello stesso file (**_cordova-js/package.json_**) modificare la versione di **execa** e **fs-extra**, così come riportato di seguito:
  ```javascript
    "dependencies": {
        "execa": "^3.4.0",
        "fs-extra": "^8.1.0",
        ...
      },
  ```
  7. Entrare in ogni cartella di **_SmartTV-Dev_** ed eseguire il seguente comando per installare le dipendenze:
  ```bash
  npm install
  ```
  8. Entrare in **_cordova-js_** ed eseguire il seguente comando per compilare:
  ```bash
$ grunt compile:sectv-orsay compile:sectv-tizen compile:tv-webos
  ```
  9. Allo stesso modo, entrare in **_cordova-plugin-toast_** ed eseguire il seguente comando per compilare:
  ```bash
$ grunt compile:sectv-orsay compile:sectv-tizen compile:tv-webos
  ```
  10. Entrare nella directory principale **_SmartTV-Dev_** e clonare questo progetto cordova, poi entrare nel progetto:
  ```bash
$ git clone https://github.com/Boncoddo/laMiaTV.git
$ cd laMiaTV
  ```
  11. (a) Eseguire la copia dei file per il sistema e compilare (per windows):
  ```bash
$ xcopy ..\grunt-cordova-sectv\sample\* .\ /y /s
$ npm install ../grunt-cordova-sectv
  ```
  11. (b) Eseguire la copia dei file per il sistema e compilare (per sistemi unix-like):
  ```bash
$ cp -rf ../grunt-cordova-sectv/sample/. ./
$ npm install ../grunt-cordova-sectv
  ```
  12. Per installare le dipendenze del progetto eseguire:
  ```bash
$ npm install
  ```
  13. Per poter simulare l'applicazione nel browser eseguire:
  ```bash
$ cordova platform add browser
  ```
  14. Plugin obbligatori per l'utilizzo del simulatore del browser (non per altre piattaforme):
  ```bash
$ cordova plugin add cordova-plugin-device
$ cordova plugin add cordova-plugin-network-information
$ cordova plugin add cordova-plugin-globalization
  ```
  15. Aggiungere plugin TOAST:
  ```bash
$ cordova plugin add ../cordova-plugin-toast
  ```


## Configurazione

Per impostare una propria lista canali bisogna entrare nella cartella **_lista_** e modificare il file **_link.txt_**, avendo l'accortezza di sostituire il link fornito di default con il proprio, è anche possibile linkare una lista locale inserendo l'IP del server privato con relativo numero di porta.

```
# Inserisci il link alla lista .M3U
# Es. http://192.168.1.234:80/esempio.m3u

http://www.pandasat.info/iptv/kodi
```

La lista canali predefinita è fornita da **PandaSat**.


## Compilazione ed esecuzione da browser

1. Eseguire il comando per compilare dalla cartella del progetto:

```bash
$ cordova build browser
```

2. Avviare l'applicazione con il comando:

```bash
$ cordova emulate browser
```

Se non funziona, provare con una porta diversa:
```bash
$ cordova emulate browser --port=8080
```

*Nel caso si visualizzasse nel browser un messaggio di "Errore nel caricamento", controlla di aver disabilitato le restrizioni sulle origini multiple.*


## Compilazione ed esecuzione su Samsung Orsay

1. Entrare nella cartella del progetto ed eseguire il comando per preparare la compilazione:

```bash
$ grunt sectv-prepare:sectv-orsay
```

2. Premere il tasto '**Y**' ed il tasto **Invio** per confermare le scelte di default.

3. Eseguire il comando per compilare:

```bash
$ grunt sectv-build:sectv-orsay
```

4. Installare Apache:
[Clicca qui per entrare nel sito ufficiale e procedere al download](https://httpd.apache.org/download.cgi)

5. Copiare il file compilato con estensione **_.zip_** contenuto nella cartella del progetto **_platforms\sectv-orsay\build_** nella cartella **_htdocs\Widget_** di Apache.
_Nota: la cartalla **_Widget_** non esiste e va creata._

6. Creare un nuovo file in **_htdocs_** chiamato **_widgetlist.xml_** ed inserire il seguente contenuto:
```xml
<?xml version="1.0" encoding="UTF-8"?>
<rsp stat="ok">
<list>
    <widget id="laMiaTV">
        <title>laMiaTV</title>
        <compression type="zip" size="xxxxx"/>
        <description/>
        <download>http://xxx.xxx.xxx.xxx/Widget/laMiaTV.zip</download>
    </widget>
</list>
</rsp>
```
Dove in **size** bisogna inserire la dimensione in byte del pacchetto **.zip** compilato, dopodiché al posto di **xxx.xxx.xxx.xxx** inserire l'indirizzo IP privato del proprio PC.

7. Avviare il server **Apache** sul PC, per farlo è possibile servirsi di **ApacheMonitor** (**_bin\ApacheMonitor_**).

8. Installare l'app nel televisore:
- [Guida per l'installazione su TV Samsung del 2011](https://developer.samsung.com/smarttv/develop/legacy-platform-library/art00013/index.html#developer-login) (in inglese)
- [Guida per l'installazione su TV Samsung del 2013](https://developer.samsung.com/smarttv/develop/legacy-platform-library/d20/index.html#login-to-user-app-and-install-an-application) (in inglese)
- [Guida per l'installazione su TV Samsung del 2014](https://developer.samsung.com/smarttv/develop/legacy-platform-library/art00121/index.html#login-to-user-app-and-install-an-application) (in inglese)


## Compilazione ed esecuzione su Samsung Tizen

1. Entrare nella cartella del progetto ed eseguire il comando per preparare la compilazione:

```bash
$ grunt sectv-prepare:sectv-tizen
```

2. Premere il tasto '**Y**' ed il tasto **Invio** per confermare le scelte di default.

3. Per compilare bisogna prima impostare le variabili d'ambiente:

- Nel caso in cui è stato installato **Tizen TV SDK 2.4** digitare il comando (per windows):

```bash
$ SET PATH=%PATH%;C:\tizen-sdk\tools\ide\bin
```

- Invece, nel caso in cui è stato installato **Tizen Studio** digitare il comando (per windows):

```bash
$ SET PATH=%PATH%;C:\tizen-studio\tools\ide\bin
```

_Nota: per un sistema operativo diverso da windows fare riferimento a comandi equivalenti._

4. Modifica **_Gruntfile.js_** e imposta _profilePath_ e _profileName_:

```javascript
    'sectv-Tizen': { 
        profilePath: 'C\:\\tizen-studio-data\\profile\\profiles.xml', 
        profileName: '<tuoNomeProfilo>', 
        www: 'platforms/sectv-tizen/www',
        dest: 'platforms/sectv-tizen/build'
    }
```

- Per impostare il tuo _profilePath_:
    - Nel caso in cui è stato installato **Tizen TV SDK 2.4**:
**_<tuoWorkspace>\\.metadata\\.plugins\\org.tizen.common.sign\\profiles.xml_**
    - Nel caso in cui è stato installato **Tizen Studio**:
 **_C:\\tizen-studio-data\\profile\\profiles.xml_** (come da esempio)


- Per impostare il tuo _profileName_:
    - Nel caso in cui è stato installato **Tizen TV SDK 2.4**:
    **window > Preferences > Tizen SDK > Security Profiles**
    - Nel caso in cui è stato installato **Tizen Studio**:
    **Tools > Certificate Manager > Certificate Profile**

5. Eseguire il comando per compilare:

```bash
$ grunt sectv-build:sectv-tizen
```

6. Entrare su **_platforms\sectv-tizen\www\build_** e importare il file **_.wgt_** su **Tizen Studio**.

7. Installare l'app nel televisore:
- [Guida per l'installazione su TV Samsung con Tizen](https://developer.samsung.com/smarttv/develop/getting-started/using-sdk/tv-device.html#connecting-the-tv-and-sdk) (in inglese)


## Compilazione ed esecuzione su LG WebOS

1. Entrare nella cartella del progetto ed eseguire il comando per preparare la compilazione:

```bash
$ grunt sectv-prepare:tv-webos
```

2. Premere il tasto '**Y**' ed il tasto **Invio** per confermare le scelte di default.

3. Per compilare bisogna prima impostare le variabili d'ambiente (per windows):

```bash
$ SET PATH=%PATH%;C:\webOS_TV_SDK\CLI
```

_Nota: per un sistema operativo diverso da windows fare riferimento a comandi equivalenti._

4. Eseguire il comando per compilare:

```bash
$ grunt sectv-build:tv-webos
```

5. Installare l'app nel televisore, tenendo presente che il file **_.ipk_** compilato si trova in **_platforms\tv-webos\build_**:
- [Guida per l'installazione su TV LG con WebOS](http://webostv.developer.lge.com/sdk/tools/using-webos-tv-cli/testing-web-app-cli/) (in inglese)


## Licenza

Questo progetto è concesso in licenza in base ai termini della **GNU General Public License v3.0**.


## Note dello sviluppatore

Tenete presente che l'applicazione è stata testata su una **TV Samsung Orsay del 2014**, dunque non è detto che possa funzionare anche su altri ambienti.


## Disclaimer

L’uso dell'applicativo per la visione di programmi protetti da diritto d'autore non è consentito dalla legge, si declina qualsiasi responsabilità relativa all'utilizzo improprio del presente software.