# Rython-parser am 05. Feb. 2020.

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
