# Просопографический анализ протоколов заседаний конференции Императорской академии наук (1725 - 1743 гг.)

# Prosopographische Analyse der Sitzungsprotokolle der Konferenz der Kaiserlichen Akademie der Wissenschaften (1725 - 1743)

# Prosopographic analysis of the protocols of the meetings of the conference of the Imperial Academy of Sciences (1725 - 1743)


## Beginning. [Johann Jährig](https://en.wikipedia.org/wiki/Johannes_J%C3%A4hrig) 


![](https://github.com/alexdyul/Jahresarbeit_Kursowaja/blob/master/preza/Pallas%2C%20Katalognummer%2078509.JPG)



## Who were the full members of the Russian Academy of Sciences in the 20s and 40s of the 18th century and what collective characteristics did they have??



[Regulations on the establishment of the Academy of Sciences and Arts. 1724](http://arran.ru/data/pdf/projpol.pdf):
### Class 1 - Mathematical Sciences. It was divided into 3 categories:
• Arithmetic, algebra and geometry
• Astronomy, geography, navigation
• Mechanics
### Class 2 - Physical Sciences. It was divided into 4 categories:
• Theoretical and experimental physics
• Anatomy
• Chemistry
• Botany
### Class 3 - Humanities. It was divided into 3 categories:
• Oratory of antiquity
• Ancient and modern history
• Private and public law, politics and ethics


## Characterization of the initial digital objects

1.	[Scans](http://ranar.spb.ru/rus/protokol1/id/233/) of the protocols of the Academy of Sciences for 1725-1743 are available to us. Site of the Russian Academy of Sciences. Publication of 1897 of average quality. Protocol languages:
•	Latin from November 13, 1725 to November 08, 1733;
•	German from November 11, 1734 to December 31, 1741;
•	Latin from January 9, 1742 to December 23, 1743.

![](https://github.com/alexdyul/Jahresarbeit_Kursowaja/blob/master/preza/protokols.png)

2.	[Scan of the book](https://www.prlib.ru/item/451270) “List of members of the Imperial Academy of Sciences, 1725 - 1907”, edited by Boris Lvovich Modzalevsky. Publication of 1908 of average quality. Website of the Russian Presidential Library.

![](https://github.com/alexdyul/Jahresarbeit_Kursowaja/blob/master/preza/list_akad.png)

3.	[The database](http://isaran.ru/?q=ru/persostav) of the personal composition of the Russian Academy of Sciences. The site of the "Information System Archives of the Russian Academy of Sciences".

4.	[Eric Amburger database](https://www.amburger.ios-regensburg.de/) "Foreigners in pre-revolutionary Russia." Website of the Institute for Eastern and Southeast European Studies named after Leibniz.




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

