# PongGame-Arduino 

Acesta este un joc Pong simplu implementat pe o placă Arduino folosind un afișaj OLED SSD1331.

## Componente necesare

- Placă Arduino compatibilă (ex. Arduino UNO)
- Afișaj OLED color SSD1331 96x64 pixeli
- 2 butoane pentru controlul paletei jucătorului
- Fire pentru conexiuni
- Breadboard (opțional)

## Conectare

- Pinii SCLK, MOSI, CS, RST, DC ai afișajului se conectează la pinii Arduino configurați în cod
- Butoanele se conectează între pinii digitali 2 și 3 și GND, cu rezistențe pull-up activate intern

## Biblioteci necesare

- SPI.h
- Wire.h 
- Adafruit_GFX.h
- Adafruit_SSD1331.h

## Funcționalitate

- La pornire se afișează un ecran cu titlul jocului. Se apasă orice buton pentru a începe.
- Jocul se desfășoară între jucător (paleta verde) și calculator (paleta roșie). 
- Jucătorul controlează paleta cu butoanele Sus și Jos.
- Mingea ricoșează de pereți și palete, schimbându-și direcția.
- Când se înscrie un punct, se afișează scorul curent pentru 2 secunde.
- Jocul se termină când un jucător ajunge la 5 puncte. Se afișează câștigătorul.
- Se poate porni un nou joc apăsând orice buton.

## Personalizare

În cod se pot modifica ușor:
- Pinii Arduino folosiți pentru afișaj și butoane  
- Viteza jocului prin constantele PADDLE_RATE și BALL_RATE
- Înălțimea paletelor prin constanta PADDLE_HEIGHT
- Scorul maxim pentru câștigarea jocului prin variabila MAX_SCORE

## Iată câteva potențiale probleme și vulnerabilități pe care le are codul Arduino pentru jocul Pong:

1. **Blocarea buclei principale**
   - În funcția `loop()`, dacă una dintre condițiile de timp (`time > ball_update` sau `time > paddle_update`) nu este îndeplinită niciodată din cauza unei probleme de timing, bucla se poate bloca și jocul nu va mai răspunde.
   - Pentru a evita asta, ai putea adăuga un timeout sau o condiție de ieșire de rezervă.

2. **Depășirea limitelor ecranului**
   - Deși codul are implementate verificări pentru a menține paleta jucătorului și cea a calculatorului în interiorul ecranului, mingea poate ieși în anumite cazuri rare (de ex. la colțuri).
   - Posibilă soluție: Verifică întotdeauna dacă noile coordonate x și y ale mingii sunt în limite valide înainte de a le folosi.

3. **Divizare la zero**
   - În codul furnizat nu sunt cazuri de divizare la zero, dar în general trebuie avut grijă când se folosesc valori variabile la numitor, validându-le întâi.

4. **Buffer overflow**
   - Matricele folosite pentru grafică (`pong[]` și `game[]`) par a fi definite static cu dimensiuni fixe, deci aici nu e risc de overflow.
   - Dar în general, când se folosesc buffere, trebuie validată dimensiunea datelor copiate în ele față de dimensiunea alocată, pentru a evita depășirea și coruperea memoriei.

5. **Valori hardcodate**
   - Pinii Arduino, culorile, coordonatele, ratele de update, scorurile sunt în general definite ca valori hardcodate. 
   - Ar fi mai flexibil și mentenabil pe termen lung să se definească aceste valori ca variabile sau constante la începutul codului, cu nume sugestive.

6. **Tratarea eronată a intrărilor**
   - Butoanele sunt considerate apăsate când pinul respectiv este LOW, dar dacă cumva un pin rămâne flotant, poate genera apăsări false.
   - Ideal ar fi validarea stării butoanelor de ex. verificând de 2 ori la un interval scurt, pentru a elimina zgomotul și apăsările accidentale.

7. **Lipsa comentariilor și documentației**
   - Chiar dacă funcționalitatea codului poate fi evidentă acum, pe termen lung sau pentru alți dezvoltatori ar fi util să existe comentarii pentru a explica scopul și funcționarea fiecărei secțiuni de cod.

Distractie placuta cu acest joc Arduino Pong!
