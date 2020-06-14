№ R code for data analysis and visualization

```R
library("tidyverse")
library("readxl")

list_person <- read_xlsx("C:/R/Kurs/list_person.xlsx")
list_person %>% 
  ggplot(aes(fct_reorder(name, mid_year_frequency), mid_year_frequency)) +
    labs(x = "",
       y = "",
       title = "Mid year frequency")+
  geom_col() +
  coord_flip()

list_person %>% 
  ggplot(aes(fct_reorder(name, age_receipt), age_receipt)) +
  labs(x = "",
       y = "",
       title = "Age receipt")+
  geom_col() +
  coord_flip()

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
