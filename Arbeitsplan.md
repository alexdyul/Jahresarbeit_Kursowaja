# Jahresarbeit_Kursowaja

## Thema der Arbeit

Просопографический анализ протоколов заседаний конференции Императорской академии наук (1725 - 1743 гг.)

Prosopographische Analyse der Sitzungsprotokolle der Konferenz der Kaiserlichen Akademie der Wissenschaften (1725 - 1743)

Prosopographic analysis of the protocols of the meetings of the conference of the Imperial Academy of Sciences (1725 - 1743)

### Schwerpunkte

- die nötigen Handgriffe zu lernen / Arbeit mit Scan und Text ([OCR4all](https://github.com/OCR4all), XML, TEI) 

- Technologie der Analyse auszusuchen / Arbeit mit Daten (Python, R) 

- Datenanalyse theoretisch auszuführen / Arbeit mit Literatur und Archiven   

### Welhe Daten sind jetzt erreichbar?

- Scan von alle Protokollen. [Auf lateinisch und deutsch](http://ranar.spb.ru/rus/protokol1/id/233/)

- Scan von dem Buch ["Liste der Mitglieder der kaiserlichen Akademie der Wissenschaften, 1725-1907"](https://www.prlib.ru/item/451270)

- Datenbank ["Personalbestand von Akademie der Wissenschaften"](http://isaran.ru/?q=ru/persostav) 

- Erik-Amburger-Datenbank ["Ausländer im vorrevolutionären Russland"](https://www.amburger.ios-regensburg.de/)

### Generalplan

- Parsing von den Protokollen (bis 1. Dez. 2019)  **Gemacht mit Rython-parser am 05. Feb. 2020. Es war sehr kompliziert** 

```python
import requests
from bs4 import BeautifulSoup as bs

headers = {"Accept": "*/*",
           "User-Agent": "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/70.0.3538.102 Safari/537.36 Edge/18.18362"}

b_url = "http://ranar.spb.ru/rus/protokol1/id/233"

def protokol ():
    url = []
    sesson = requests.Session()
    request = sesson.get(b_url, headers = headers)
    if request.status_code == 200:
        soup = bs(request.content, "lxml")
        table = soup.find_all("a", attrs={"class": "zoom"})
        for foto in table:
            url.append ("http://ranar.spb.ru" + foto["href"])
    else:
        print ("error")
    return (url)
    
def get_file(url):
    # print(url)
    response = requests.get(url, stream=True)
    return response

def save_data(name, file_data):
    file = open(name, 'bw')
    for chunk in file_data.iter_content(4096):
        file.write(chunk)

def get_name(url):
    name = url.split('/')[-1]
    return name
    
def main ():
    for name in protokol():
        save_data(get_name(name),get_file(name))

if __name__=="__main__":
    main()
```

- Verarbeitung den Skans in ~~OCR4all~~ (bis 1. Feb. 2020)  **Gemacht mit Abbyy_FinerReader_15 am 06. Feb. 2020. Mindestens 3 % Fehler. Es war sehr leicht**

- Identifizierung von Personen im Namensregister (bis 1. März 2020)

- Korrelation den prosopographische Daten von andere Datenbanken und Bücher (bis 1. Apr. 2020)

- Datenanalyse und Zusammenfassung der Jahresarbeit (bis 1. May 2020)

## Jede Woche ist kurzer Bericht nötig

## Hauptfrage
Wer waren inoffizielle (latente) Mitglieder von Russische Akademie der Wissenschaften in 20 - 40 Jahren XVIII (1725 - 1743) und welche Charakteristik hatten sie?
