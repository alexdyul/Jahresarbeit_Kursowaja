# Просопографический анализ протоколов заседаний конференции Императорской академии наук (1725 - 1743 гг.)

# Prosopographische Analyse der Sitzungsprotokolle der Konferenz der Kaiserlichen Akademie der Wissenschaften (1725 - 1743)

# Prosopographic analysis of the protocols of the meetings of the conference of the Imperial Academy of Sciences (1725 - 1743)





## Beginning. [Johannes Jährig](https://en.wikipedia.org/wiki/Johannes_J%C3%A4hrig) 


![](https://github.com/alexdyul/Jahresarbeit_Kursowaja/blob/master/preza/Pallas%2C%20Katalognummer%2078509.JPG)



### [Regulations on the establishment of the Academy of Sciences and Arts. 1724](http://arran.ru/data/pdf/projpol.pdf):

#### Class 1 - Mathematical Sciences. It was divided into 3 categories:
• Arithmetic, algebra and geometry
• Astronomy, geography, navigation
• Mechanics
#### Class 2 - Physical Sciences. It was divided into 4 categories:
• Theoretical and experimental physics
• Anatomy
• Chemistry
• Botany
#### Class 3 - Humanities. It was divided into 3 categories:
• Oratory of antiquity
• Ancient and modern history
• Private and public law, politics and ethics





## Problem 1
On the Academy’s official page it is indicated that in 1746 the first Russian president of the Academy was appointed and after that the Russian members of the Academy began to be elected. The first of them were S.P. Krasheninnikov and M.V. Lomonosov. The years of election were 1745 and 1742.

![](https://github.com/alexdyul/Jahresarbeit_Kursowaja/blob/master/preza/acad_hist.png)




### Really?

![](https://github.com/alexdyul/Jahresarbeit_Kursowaja/blob/master/preza/adadurov.png)
Screenshot. [Modzalevsky, 1908]





## Problem 2
In addition, the academy’s page does not indicate the country where the academy members came from, but says that they were “foreigners”. At the same time, for many historians, the predominance of scientists from German lands in the Russian Academy of Sciences is obvious, and there is nothing “anti-Russian” in this fact.





### Our target group.
In this work, we decided to take for the target group a clearly limited category of “full members of the Academy of Sciences”.
This category is limited to 48 persons in the period from 1725 to 1743. We can say this with confidence by referring to the book “List of members of the Imperial Academy of Sciences, 1725 - 1907” (edited by Boris Lvovich Modzalevsky, 1908).





## Main question. Who were the full members of the Russian Academy of Sciences in the 20s and 40s of the 18th century and what collective characteristics did they have?





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






## Creation of Python code for parsing protocol images

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

## OCR data recognition in Abbyy FinerReader of 15 images of all protocol pages


## Convert the image recognized in OCR to [text format](https://github.com/alexdyul/Jahresarbeit_Kursowaja/blob/master/protocols.txt)


## Detection of objects such as human. At this stage, it was difficult to identify some people, as we talked about earlier. 



### Problem 

The original protocols
![](https://github.com/alexdyul/Jahresarbeit_Kursowaja/blob/master/preza/Kohl.png)

Text corpora of the protocols
![](https://github.com/alexdyul/Jahresarbeit_Kursowaja/blob/master/preza/kohl1.png)


#### Prof. De l’lsle

A search using a regular expression language such as “Nicol. * Lsle”, “Nicol. {6} lsle” and others, allowed to identify 130 and 122 cases for each person. At the same time, a search by the name “lsle” reveals 425 matches. 

Thus, more than a third of cases could not be identified due to the fact that often in the protocols “Prof. De l’lsle. " Since both brothers were professors, this makes final identification impossible without a detailed reading of the protocol context.



#### OCR errors is from 2 to 4 %

In the case of Du Vernoi, the use of regular expressions revealed 3.15% of recognition errors and increased the number of matches from 369 to 381.

In the case of Bayer, regular expressions revealed 2.17% more matches (from 812 to 830).

This and other examples allow us to make the assumption that the percentage of errors in the case of other surnames is from 2 to 4 %.





## Collection. Aggregation of discovered resources in a structured way. [table XLSX](https://github.com/alexdyul/Jahresarbeit_Kursowaja/blob/master/list_person.xlsx).

![](https://github.com/alexdyul/Jahresarbeit_Kursowaja/blob/master/preza/tibble.png)

• year_of_birth

• year_of_death

• lifetime

• year_receipt. Year of admission to the academy.

#### • age_receipt. The age at which the person was admitted to the academy.

• frequency. Frequency of references to the person in the text corpora of the protocols.

• activ_time. The number of years of stay as a member of the academy in the period from 1725 - 1743.

#### • mid_year_frequency. The average frequency of mentions of a person per year. It is considered as frequency/activ_time. This is the only metric that allows you to indirectly talk about the activity of participation in the activities of the Academy.

#### • country_of_birth. In the case of German lands, the conditional generalization “Germany” was used.

• country_of_death. In the case of German lands, the conditional generalization “Germany” was used.

#### • specialty. The branch of knowledge with which the person entered the academy and was most closely associated.

#### • study. One of the three classes of sciences in accordance with the rules of the Academy of 1724. Mathematical, physical, humanitarian.

• link. Link to the individual page in The database of the personal composition of the Russian Academy of Sciences.



## Visualization. All the analytical work and visualization we carried out in the programming language R in the program Rstudio.




### The simplest and most obvious thing that we could analyze is the origin by country as a percentage.

```R
library("tidyverse")
library("readxl")

list_person <- read_xlsx("C:/R/Kurs/list_person.xlsx")

list_person %>% 
  group_by(country_of_birth) %>%
  count(country_of_birth, sort = TRUE) %>%
  mutate(proz = n/48*100) %>%
  ggplot(aes(fct_reorder(country_of_birth, proz), proz, label = proz)) +
  labs(x = "",
       y = "",
       title = "Сountry of birth (%)")+
  geom_col() +
  geom_text(nudge_y = 10)+
  coord_flip()
```

![](https://github.com/alexdyul/Jahresarbeit_Kursowaja/blob/master/preza/country.png)





### The average age at which persons were admitted to the academy was 30.66 years.  

```R
list_person %>% 
  ggplot(aes(fct_reorder(name, age_receipt), age_receipt)) +
  labs(x = "",
       y = "",
       title = "Age receipt")+
  geom_col() +
  coord_flip()
```

![](https://github.com/alexdyul/Jahresarbeit_Kursowaja/blob/master/preza/average%20age.png)





### Visualization of the average annual frequency in the text corpora of the protocols shows us several "outliers". 

```R
list_person %>% 
  ggplot(aes(fct_reorder(name, mid_year_frequency), mid_year_frequency)) +
    labs(x = "",
       y = "",
       title = "Mid year frequency")+
  geom_col() +
  coord_flip()
```

![](https://github.com/alexdyul/Jahresarbeit_Kursowaja/blob/master/preza/average%20annual%20frequency.png)






### If we calculate the distribution of academy members by field of knowledge, then in the classification of 1724 we get absolute equality.

```R
list_person %>% 
  group_by(study) %>%
  count(study, sort = TRUE) %>%
  mutate(proz = n/48*100) %>%
  ggplot(aes(fct_reorder(study, proz), proz, label = proz)) +
  labs(x = "",
       y = "",
       title = "Study (%)")+
  geom_col() +
  geom_text(nudge_y = 10)+
  #coord_flip()
```

![](https://github.com/alexdyul/Jahresarbeit_Kursowaja/blob/master/preza/field%20of%20knowledge.png)




```R
list_person %>% 
  group_by(specialty) %>%
  count(specialty, sort = TRUE) %>%
  mutate(proz = n/48*100) %>%
  ggplot(aes(fct_reorder(specialty, proz), proz, label = proz)) +
  labs(x = "",
       y = "",
       title = "Specialties (%)")+
  geom_col() +
  geom_text(nudge_y = 5)+
  coord_flip()
```

![](https://github.com/alexdyul/Jahresarbeit_Kursowaja/blob/master/preza/specialties.png)

 




### We decided to create a general picture in the interconnection of the categories “mid_year_frequency” and “age_receipt”. 
We painted the names in the category “study”
We used the geom_smooth method to Add a smoothed conditional mean.

```R
library("ggrepel")

list_person %>%
mutate(age_receipt = as.integer(age_receipt)) ->
  df

df %>% 
  ggplot(aes(age_receipt, mid_year_frequency, label = name))+
  geom_smooth(method = "lm", se = FALSE)+
  geom_point(aes(color = study))+
  ggrepel::geom_text_repel(aes(color = study))
```

![](https://github.com/alexdyul/Jahresarbeit_Kursowaja/blob/master/preza/geom_smooth.png)





## Conclusion


### In 60.42% of cases, academics came from German lands. This percentage can be adjusted upwards by introducing the category “nationality”. Many academics born in Russia come from German families.


### The average age for admission to the academy was 30.66 years. This, of course, reflects the reality of the first half of the 18th century, when the average life expectancy of academics was 56.94 years.


### Equal distribution of the number of academicians by scientific classes. 16 people, or 33% of the total for mathematical, physical and human sciences.


### The most popular specialties of academicians are mathematics and history, 18.75% of the total number of specialties.


### There is a slight trend: the more adult person becomes an academician, the higher his performance in the activities of the academy.


### The average “mid_year_frequency” was 24.28 references per year. Of the 16 humanities, only 3 academicians were able to overcome this bar. Among mathematicians and physicists, 7 academicians exceeded the average value. However, this circumstance did not affect the distribution of the number of academicians by class of science.




# Thus, the "average portrait" of a full member of the Russian Academy of Sciences in the period from 1725 to 1743 looks like this: a native of Germanic lands, he was accepted to the academy at the age of 30 with a degree in mathematics or history.

![](http://isaran.ru/isaran/image2.php?guid=8ED13652-56FA-AF50-298D-D2167A795E85)
[Bayer Gottlieb Siegfried](http://isaran.ru/?q=ru/person&guid=2227570B-250E-6A77-25BA-CC4FA8AEE383&str=%D0%91%D0%B0%D0%B9%D0%B5%D1%80)





