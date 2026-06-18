# Voice changer
Acest proiect generează un semnal vocal (sintetizat) pe baza semnalului vocal captat la intrare pe baza fonemelor. 
Proiectul conține un set de sunete înregistrate (care pot fi inlocuite) pe baza cărora se generează vocea artificială.
Pentru a detecta fonemele captate, se folosec algoritmi de clasificare bazați pe rețele neuronale, ce trebuie adaptați pentru vocea persoanei, cat si pe răspunsul microfonului.
Pentru un rezultat dorit, se recomanda ca dispozitivul de captare sa fie izolat de dispozitivul de reproducere a sunetului, cât și de surse de zgomot externe.

## (Instalare)
După cum a fost mentionat anterior, pentru cele mai bune rezultate, e important ca rețeaua neuronală sa fie antrenată pe vocea utilizatorului.
Pentru antrenare, se pornește ADC-ul și se reglează nivelul de intrare.
Dupa aceasta, se deschid proiectele "WekinatorProjectConsoane" și "WekinatorProjectVocale" și se sterg datele salvate (sunt adaptate pentru creator), după care se începe procesul de adaptare.
Pentru a adapta vocalele la vocea utilizatorului, se procedeaza in felul urmator:
1: Se seteaza toate iesirile ale proiectului "WekinatorProjectVocale" la 0
2: Se seteaza valoarea maxima pentru iesirea corespunzatoare vocalei pe care se face antrenarea, anume:
  iesirea 1 pentru vocala 'a'
  iesirea 2 pentru vocala 'e'
  iesirea 3 pentru vocala 'i'
  iesirea 4 pentru vocala 'o'
  iesirea 5 pentru vocala 'u'
  iesirea 6 pentru vocala 'î'
  iesirea 7 pentru vocala 'ă'
3: Se pronunță vocala în micorfon și se da comanda de incepere a înregistarii ("start recording") in wekinator
4: Dupa un număr rezonabil (de ex. 70) de date înregistrate, se opreste inregistrarea
5: Se repeta pasul 1 pentru fiecare vocală suportata (in total, de 7 ori)
6. Se apasa pe ‘train’ pentru a intrena reteaua

Pentru a consoanele la vocea utilizatorului, se procedeaza in felul urmator:
1: Se seteaza toate ieșirile ale proiectului "WekinatorProjectVocale" la 0
2: Se seteaza valoarea maxima pentru iesirea la valoarea corespunzatoare conosanei pe care se face antrenarea, anume:
  valoarea 01 pentru sunetul 'b'
  valoarea 02 pentru sunetul 'ci'
  valoarea 03 pentru sunetul 'd'
  valoarea 04 pentru sunetul 'f'
  valoarea 05 pentru sunetul 'gh'
  valoarea 06 pentru sunetul 'gi'
  valoarea 07 pentru sunetul 'ch'
  valoarea 08 pentru sunetul 'l/r'
  valoarea 09 pentru sunetul 'm'
  valoarea 10 pentru sunetul 'n'
  valoarea 11 pentru sunetul 'p'
  valoarea 12 pentru sunetul 's'
  valoarea 13 pentru sunetul 'ș'
  valoarea 14 pentru sunetul 't'
  valoarea 15 pentru sunetul 'ț'
  valoarea 16 pentru sunetul 'v'
  valoarea 17 pentru sunetul 'z'
3: Se dă comanda de începere a înregistarii ("start recording") în wekinator și se pronunță consoana in micorfon
4: Imediat se opreste înregistrarea (în special pentru consoanele surde) (pentru a nu inregistra zgomotul)
5: Se repeta pasul 1 pentru fiecare consoană suportată (in total, de 17 ori)
6. Se apasa pe ‘train’ pentru a intrena reteaua

## (Utilizare)
Butonul ADC pornește intrarea, cât si ieșirea audio pentru acest patch.
Butonul "Vocloop" pornește mecanismul de redare a vocalelor.
Pentru utilizare, cele 2 butoane mentionate anterior trebuie sa fie pornite.
Slider-ul "nivel intrare" permite amplificarea/reducerea din nivelul de intrare. De ținut cont, că la pornire, acesta este initializat cu valoarea 0.
Slider-ul "nivel conoane" permite amplificarea/reducerea din nivelul sunetelor corespunzătoare consoanelor înregistrate pentru a facilita detectarea fonemelor. De ținut cont, că la pornire, acesta este initializat cu valoarea 0.5 (-6dB).
Mai jos sunt 2 ecrane nivelul fiecărui dintre coeficienți trimiși către wekinator și nivelul la care cele 2 tipuri de foneme au fost captate.

## (Istoric)

(28.05) Prima varianta care putea reproduce doar 5 vocale dupa un algoritm de clasificare bazat pe retele neuronale. Avea dezavantajul ca 

(02.06) Varianta modificata care reproduca vocalele dupa un algoritm continuu ce returna probabilitatea a fiecareia dintre vocale bazat pe retele neuronale 

## (Link-uri)

https://ro.wikipedia.org/wiki/Fonem
https://www.telecom.pub.ro/

# Dezvoltarea proiectului

Proiectul a fost inspirat dintr-un proiect mai vechi in anul 3 in care se incerca reproducera vocei pe un dispozitiv cu memorie foarte limitata, pornind de la teoria de la cursul de prelucrare a semnalelor digitale, tot din anul 3 + teoria de codare a semnalelor vocale,
anume ideea e că doar vocalele (foneme sonore) au frecvență fundamentală, iar detecția sunetelor se face la aproximativ un interval de 10..30 ms.
Primele vocodere se bazau pe această idee, si transmiteau doar frecvența fundamentală, tipul de sunte și parametrii filtrelor. Generarea vocei se făcea în funcție de tipul de fonem (sonor/insonor).
Dacă fonemul era sonor, atunci se genera un semnal în formă de fierestrău, trecut printr-un filtru trece jos și printr filtre de tip peak, care, în funcție de parametri, generau un sunet asemănător cu sunetul (vocala) generat de vorbitor.
În cazul fonemelor insonore, semnalul de bază era zgomot alb, iar filtrele de tip peak (și FTJ, dacă mai era) aveau frecvență mai înaltă, după 2000 Hz.
În acet proiect abordarea este puțin diferită, anume sunetele nu sunt generate de un semnal funție, ci de sunete preînregistrate (de dorit cu aceeași frecvență fundamentală) ce corespund fonemelor ce vor să fie sintetizate.
Pornind de la ideea că doar la vocale e importantă frecvența fundamentală, doar sunetele corespunzătoare acestor foneme vor fi reproduse în funcție de frecvența de bază a vorbitorului. Deoarece lungimea este nedeterminată, sunetele se repetă.
În cazul fonemelor insonore, algoritmul va reproduce sunetele la frecvența înregistrată și fără repetare.

În practică, acest algoritm se paote comporta bine doar dacă vorbitorul vorbește clar + camera este izolată de sunete nedorite.
Pentru a mări numărul de foneme sonore reproduse + a face mai neobservabilă trecerea de la un fonem la altul (de ex interjecția „ei!”), se reproduc toate sunetele simultant, cu ponderea cea mai mare spre fonemul detectat.
Cred ca ar putea fi îmbunătățit prin detecția mai acurată a fonemelor sau, după cum a fost văzut inițial, detectarea de combinații vocală + consoană și reproducerea sunetului ce va conține combinația.

