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

När vi undersöker .pcap-filen ser vi att någon har surfat på internet och tittat på hemsidor. All den trafiken är skyddad med [TLS](https://en.wikipedia.org/wiki/Transport_Layer_Security) och det finns därför inget (enkelt) sätt att läsa den trafiken.

När vi tittar vidare i filen hittar vi en del FTP-trafik, den vill vi titta närmre på.
</p>
</details>

<details><summary>Tryck för att läsa</summary>
<p>

FTP, [file transfer protocol](https://en.wikipedia.org/wiki/File_Transfer_Protocol) är ett protokoll som överför filer mellan datorer. Protokollet skickar data i klartext och en aktör som kan avlyssna nätverkstrafiken kan därför ta del av informationen som skickas.

Genom att filtrerar på *ftp || ftp-data* i Wireshark kan vi se all trafik som överförts med ftp.

Det är nu enkelt att se att ftp-trafiken går i klartext och vi ser till exempel att användaren *sidden* har loggat in på ftp-servern med lösenordet *k3b4bt411rik*.

Vi ser också att användaren har laddat upp en fil med kommandot:

```
STOR demo.tar.xz
```

Vi kan också se att filen laddats upp i ett antal större paket som protokollet anget som ftp-data. Högerklickar vi på ett av de större paketen och väljer *Follow/TCP Stream* kan vi spara ned filen till våran dator.

Välj *RAW* i rullgardinsmenyn "Show and save data as" och spara sedan filen.

</p>
</details>

<details><summary>Tryck för att läsa</summary>
<p>

Port 6667 används för IRC, [Internet Relay Chat](https://en.wikipedia.org/wiki/Internet_Relay_Chat), ett protokoll för text-chat. Även IRC skickar data i klartext.

</p>
</details>
