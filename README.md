# CERT-SE-Challenge-2020-Writeup
[![License: CC BY-NC-SA 4.0](https://img.shields.io/badge/License-CC%20BY--NC--SA%204.0-lightgrey.svg)](https://creativecommons.org/licenses/by-nc-sa/4.0/)

[CERT.se](https://cert.se) släppte under cybersäkerhetsmånaden 2020 en [utmaning](https://cert.se/2020/09/cert-se-challenge-2020) på sin hemsida. Jag har skrivit ihop ett lösningsförslag och hoppas att det kan bidra till den som själv vill testa utmaningen men kanske behöver lite hjälp på traven.

Zip-filen med utmaningen återfinns även här i repot om den skulle försvinna från hemsidan.
# Lösning
Spoiler-varning, nedan beskrivs hela utmaningen. Läs inte längre än du behöver om du vill pröva utmaningen själv.
## Vad finns i zip-filen?
<details><summary>Tryck för att läsa</summary>
<p>

I zipfilen hittar du en fil med ändelsen .pcapng ett filformat som används för att spara nätverkstrafik som fångats in med till exempel [Wireshark](https://www.wireshark.org/)

Ladda ned wireshark och ta en titt i filen och se om du upptäcker något intressant.
</p>
</details>

## Vad har hänt på nätverket?

<details><summary>Tryck för att läsa</summary>
<p>

När vi öppnar pcap-filen i Wireshark kan det vara svårt att direkt se vad alla trafik som fångats in handlar om. Om vi väljer **Statistics/Conversations** får vi en översikt över trafiken i filen. Under fliken **TCP** kan vi se att det har gått trafik på bland annat port 21 och port 6697.

Det är intressant trafik som vi vill titta närmare på.

</p>
</details>

<details><summary>Tryck för att läsa</summary>
<p>

FTP, [file transfer protocol](https://en.wikipedia.org/wiki/File_Transfer_Protocol), är ett protokoll som överför filer mellan datorer. Protokollet skickar data i klartext och en aktör som kan avlyssna nätverkstrafiken kan därför ta del av informationen som skickas.

Genom att filtrerar på **ftp || ftp-data** i Wireshark kan vi se all trafik som överförts med ftp.

Det är nu enkelt att se att ftp-trafiken går i klartext och vi ser till exempel att användaren **sidden** har loggat in på ftp-servern med lösenordet **k3b4bt411rik**.

Vi ser också att användaren har laddat upp en fil med kommandot `STOR demo.tar.xz`

Vi kan också se att filen laddats upp i ett antal större paket som protokollet anget som ftp-data. Högerklickar vi på ett av de större paketen och väljer **Follow/TCP Stream** kan vi spara ned filen till våran dator.

Välj **RAW** i rullgardinsmenyn **Show and save data as** och spara sedan filen som demo.tar.xz.

</p>
</details>

<details><summary>Tryck för att läsa</summary>
<p>

Port 6697 används för IRC, [Internet Relay Chat](https://en.wikipedia.org/wiki/Internet_Relay_Chat), ett protokoll för text-chat. Även IRC skickar data i klartext.

Om vi i **Statistics/Conversations** högerklickar på trafiken som gått på port 6697 och väljer **Apply as filter/Selected/A<->B** skapar Wireshark ett sökfilter i huvudfönstret som visar trafiken mellan användaren och IRC-servern.

I huvudfönstret kan vi nu högerklicka på ett av paketen och välja **Follow/TCP Stream**, detta ger oss en lättläst sammanställning över en argumentation mellan två retrodator-entusiaster som är lite oense kring vilken retroprocessor som är den bästa.

Vi ser också att användaren som tidigare laddade upp filen berättar att koden till filen är **OC1iaXQtQzBtcHV0M2VyLXcwbmQzciE/** och att koden är "krypterad".

</p>
</details>

## Vad finns i filen?

<details><summary>Tryck för att läsa</summary>
<p>

Packar vi upp demo.tar.xz får vi en fil som heter demo.zip. Vi kan se att den innehåller filen demo.tap men när vi försöker packa upp den blir vi tillfrågade om ett lösenord. Funkar koden vi kunde läsa i IRC-konversationen?

</p>
</details>

<details><summary>Tryck för att läsa</summary>
<p>

Koden från IRC-konversationen, **OC1iaXQtQzBtcHV0M2VyLXcwbmQzciE/**, fungerar inte som lösenord för att packa upp filen. Men är lösenordet verkligen krypterat?

Efter lite funderande gissar vi att lösenordet är kodat i [base64](https://en.wikipedia.org/wiki/Base64). Vi kan snabbt googla oss fram till en [base64-konverterare](https://codebeautify.org/base64-decode) och testa.

Mycket riktigt trillar det riktiga lösenordet ut och vi kan packa upp filen.

</p>
</details>

## Push play on tape

<details><summary>Tryck för att läsa</summary>
<p>

Den uppackade .tap-filen är i ett filformat som används för att spara filer som i forntiden låg på kasettband och användes till bland annat [Commodore 64]https://en.wikipedia.org/wiki/Commodore_64).

Hitta och ladda ned en C64-emulator och kör .tap-filen så får du se ett demo som visar flaggan.

</p>
</details>

## Kan man få en kopp kaffe?

<details><summary>Tryck för att läsa</summary>
<p>

I demot finn en rullande texten som säger att att den sista ledtråden står ovan i gult, vi ser texten: <HTTP 418>

En googling på HTTP 418 leder oss till [RFC2324](https://tools.ietf.org/html/rfc2324#section-2.3.2) som var ett aprilskämt. Felmeddelande 418 I'm a teapot ska enligt RFC:n skickas av en tekanna om den ombeds brygga kaffe.

Flaggan är alltså **I'm a teapot**

</p>
</details>
