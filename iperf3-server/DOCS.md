# iperf3-server

## Käyttö

Testataksesi lähiverkkosi nopeuden iperf3-ohjelmalla, tarvitset kaksi laitetta, joista toinen toimii palvelimena tai toinen asiakaslaitteena. Kun asennat tämän lisäosan, saat Home Assistant -ympäristösi toimimaan lähiverkkosi iperf3-palvelimena. Tällöin tarvitset toisen laitteen, joka toimii asiakaskoneena.

> Huomaa, että `iperf3` on lähtökohtaisesti komentorivipohjainen ohjelma. Siihen löytyy joitakin graafisia versioita, mutta niiden käyttöä ei käsitellä tässä.

1. Lataa asiakaskoneelle iperf3-ohjelma sen [kotisivuilta](https://iperf.fr/iperf-download.php). Esimerkiksi Windows-ympäristöön voit ladata valmiiksi käännetyn version, joka on ladattavissa zip-pakettina. Lataa ja pura tämä paketti koneellesi.

2. Avaa komentorivi (PowerShell) ja siirry siihen kansioon, johon purit edellisessä vaiheessa lataamasi zip-paketin.

   Jos esimerkiksi latasit `iperf-3.16-win64.zip`-nimisen paketin ja purit sen Ladatut tiedostot -kansioon, niin silloin voit siirtyä ohjelman kansioon komennolla:

   ```sh
   cd ~\Downloads\iperf-3.16-win64
   ```

3. Testaa lähetysnopeus komennolla:

   ```
   .\iperf3.exe -c homeassistant.local
   ```

   > Lisäosan iperf3-palvelin toimii oletusportissa `5201`.

   Ohjelma tulostaa seuraavan kaltaisen tuloksen:

   ```
   Connecting to host homeassistant.local, port 5201
   [  5] local fe80::xxxx:xxxx:xxxx:xxxx port 59027 connected to fe80::xxxx:xxxx:xxxx:xxxx port 5201
   [ ID] Interval           Transfer     Bitrate
   [  5]   0.00-1.01   sec  3.88 MBytes  32.2 Mbits/sec
   [  5]   1.01-2.00   sec  4.25 MBytes  35.8 Mbits/sec
   [  5]   2.00-3.00   sec  4.88 MBytes  41.1 Mbits/sec
   [  5]   3.00-4.00   sec  4.75 MBytes  39.7 Mbits/sec
   [  5]   4.00-5.00   sec  5.00 MBytes  42.1 Mbits/sec
   [  5]   5.00-6.01   sec  4.25 MBytes  35.4 Mbits/sec
   [  5]   6.01-7.01   sec  4.38 MBytes  36.7 Mbits/sec
   [  5]   7.01-8.01   sec  3.00 MBytes  25.0 Mbits/sec
   [  5]   8.01-9.01   sec  4.12 MBytes  34.8 Mbits/sec
   [  5]   9.01-10.00  sec  4.12 MBytes  34.8 Mbits/sec
   - - - - - - - - - - - - - - - - - - - - - - - - -
   [ ID] Interval           Transfer     Bitrate
   [  5]   0.00-10.00  sec  42.6 MBytes  35.8 Mbits/sec                  sender
   [  5]   0.00-10.17  sec  42.5 MBytes  35.1 Mbits/sec                  receiver

   iperf Done.
   ```

   Oletusarvoilla `iperf3` lähettää palvelimelle kymmenen sekuntia testidataa ja tulostaa lopuksi keskimääräisen lähetysnopeuden. `sender`-rivin lukema kertoo lähetysnopeuden laitteelta palvelimelle. `receiver`-rivi puolestaan kertoo sen lukeman, jonka palvelin mittasi testin latausnopeudeksi. 

4. Testaa latausnopeus, komennolla:

   ```
   .\iperf3.exe -c homeassistant.local -R
   ```

   Tämän komennon tulos on samankaltainen kuin edellä:

   ```
   Connecting to host homeassistant.local, port 5201
   Reverse mode, remote host homeassistant.local is sending
   [  5] local 192.168.1.xxx port 59576 connected to 192.168.1.xxx port 5201
   [ ID] Interval           Transfer     Bitrate
   [  5]   0.00-1.01   sec  2.75 MBytes  23.0 Mbits/sec
   [  5]   1.01-2.00   sec  2.12 MBytes  17.8 Mbits/sec
   [  5]   2.00-3.00   sec  2.00 MBytes  16.8 Mbits/sec
   [  5]   3.00-4.00   sec  1.88 MBytes  15.7 Mbits/sec
   [  5]   4.00-5.00   sec  1.75 MBytes  14.7 Mbits/sec
   [  5]   5.00-6.01   sec  1.62 MBytes  13.6 Mbits/sec
   [  5]   6.01-7.01   sec  4.25 MBytes  35.6 Mbits/sec
   [  5]   7.01-8.01   sec  5.00 MBytes  41.9 Mbits/sec
   [  5]   8.01-9.01   sec  5.50 MBytes  46.0 Mbits/sec
   [  5]   9.01-10.01  sec  5.38 MBytes  45.2 Mbits/sec
   - - - - - - - - - - - - - - - - - - - - - - - - -
   [ ID] Interval           Transfer     Bitrate         Retr
   [  5]   0.00-10.12  sec  35.4 MBytes  29.3 Mbits/sec  996             sender
   [  5]   0.00-10.01  sec  32.2 MBytes  27.0 Mbits/sec                  receiver

   iperf Done.
   ```

   Tämän testin tuloksessa `sender`-rivi kertoo millä keskinopeudella palvelin lähetti testidatan asiakaslaitteelle ja `receiver`-rivi kertoo, millä nopeudella testidata vastaanotettiin. `retr`-sarakkeen arvo ilmoittaa kuinka monta pakettia palvelin joitui uudelleenlähettämään testin aikana. Testi tehtiin langattoman verkon kautta ja tiedonsiirrossa oli jokin lyhyt häiriö testin aikana.    