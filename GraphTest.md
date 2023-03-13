1. Считать данные из источника https://disk.yandex.ru/d/s6wWqd8Ol_5IvQ
2. Внести данные из таблицы в графовую БД
3. Построить графовое представление в БД, осуществить несколько запросов на языке запросов к графовой БД
4. Важный пункт. Найти взаимосвязи визуально и с помощью алгоритмов (алгоритмы на ваше усмотрение). 
Обратить внимание на сложные сообщества, провести анализ сложных сообществ, сделать выводы.
5. Написать rest сервис на python к графовой БД в котором на вход поступает ФИО, на выходе graphml или json


```python
"""
Считать данные из источника https://disk.yandex.ru/d/s6wWqd8Ol_5IvQ
Для загрузки данных необходимо скачать файл, так как ссылка имеет лимит скачиваний. Импортированы модули:
"""
import numpy as np
import pandas as pd
import json
```


```python
# Загрузка данных из скачанного файла с разделителем ";" и просмотр. 
df=pd.read_csv("D:/DS/data_test.csv", sep = ';')
df
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>id события</th>
      <th>ФИО участника события 1</th>
      <th>ФИО участника события 2</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>189</td>
      <td>Галчевская Карина Владимировна</td>
      <td>Белоновская Анастасия Семеновна</td>
    </tr>
    <tr>
      <th>1</th>
      <td>206</td>
      <td>Офицеров Олег Романович</td>
      <td>Сапожник Борис Валерьевич</td>
    </tr>
    <tr>
      <th>2</th>
      <td>445</td>
      <td>Жандарова Лариса Германовна</td>
      <td>Чемодуров Дамир Русланович</td>
    </tr>
    <tr>
      <th>3</th>
      <td>503</td>
      <td>Масимова Яна Дамировна</td>
      <td>Мингажетдинов Рамиль Семенович</td>
    </tr>
    <tr>
      <th>4</th>
      <td>571</td>
      <td>Мухтарова Алена Яковлевна</td>
      <td>Щербатенко Ольга Робертовна</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>4995</th>
      <td>999333</td>
      <td>Осташов Владимир Данилович</td>
      <td>Чалов Илья Владимирович</td>
    </tr>
    <tr>
      <th>4996</th>
      <td>999360</td>
      <td>Гандыбина Любовь Александровна</td>
      <td>Мерлин Илья Юрьевич</td>
    </tr>
    <tr>
      <th>4997</th>
      <td>999403</td>
      <td>Востоков Виктор Ильдарович</td>
      <td>Аликас Никита Андреевич</td>
    </tr>
    <tr>
      <th>4998</th>
      <td>999405</td>
      <td>Огарева Людмила Ильдаровна</td>
      <td>Нагайцева Алина Степановна</td>
    </tr>
    <tr>
      <th>4999</th>
      <td>999878</td>
      <td>Ряполовский Георгий Петрович</td>
      <td>Жилейкин Виктор Павлович</td>
    </tr>
  </tbody>
</table>
<p>5000 rows × 3 columns</p>
</div>




```python
# Смена названий колонок без потери смысла для удобства дальнейшей работы:
df.columns = ['event_id', 'person_1', 'person_2']
df
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>event_id</th>
      <th>person_1</th>
      <th>person_2</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>189</td>
      <td>Галчевская Карина Владимировна</td>
      <td>Белоновская Анастасия Семеновна</td>
    </tr>
    <tr>
      <th>1</th>
      <td>206</td>
      <td>Офицеров Олег Романович</td>
      <td>Сапожник Борис Валерьевич</td>
    </tr>
    <tr>
      <th>2</th>
      <td>445</td>
      <td>Жандарова Лариса Германовна</td>
      <td>Чемодуров Дамир Русланович</td>
    </tr>
    <tr>
      <th>3</th>
      <td>503</td>
      <td>Масимова Яна Дамировна</td>
      <td>Мингажетдинов Рамиль Семенович</td>
    </tr>
    <tr>
      <th>4</th>
      <td>571</td>
      <td>Мухтарова Алена Яковлевна</td>
      <td>Щербатенко Ольга Робертовна</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>4995</th>
      <td>999333</td>
      <td>Осташов Владимир Данилович</td>
      <td>Чалов Илья Владимирович</td>
    </tr>
    <tr>
      <th>4996</th>
      <td>999360</td>
      <td>Гандыбина Любовь Александровна</td>
      <td>Мерлин Илья Юрьевич</td>
    </tr>
    <tr>
      <th>4997</th>
      <td>999403</td>
      <td>Востоков Виктор Ильдарович</td>
      <td>Аликас Никита Андреевич</td>
    </tr>
    <tr>
      <th>4998</th>
      <td>999405</td>
      <td>Огарева Людмила Ильдаровна</td>
      <td>Нагайцева Алина Степановна</td>
    </tr>
    <tr>
      <th>4999</th>
      <td>999878</td>
      <td>Ряполовский Георгий Петрович</td>
      <td>Жилейкин Виктор Павлович</td>
    </tr>
  </tbody>
</table>
<p>5000 rows × 3 columns</p>
</div>




```python
copy = 0
for prob_copy_1 in df.person_1:
    for prob_copy_2 in df.person_2:
        if prob_copy_1==prob_copy_2:
            copy = copy+1
print(copy)
```

    85
    


```python
"""
Выяснилось, что один и тот же человек может присутствовать с обеих колонках по нескольку раз. 
Глядя на данные, сразу хочется посмотреть, может ли один человек учавствовать в нескольких событиях 
и может ли конкретное событие проиходить с более чем двумя людьми. Если это так, тогда значения в колонках будут повторятся.
При этом не будет пустых значений в отдельных ячейках.

"""
print ([df.nunique().sum(), df.nunique(), df.isnull().count()])
```

    [14909, event_id    4985
    person_1    4930
    person_2    4994
    dtype: int64, event_id    5000
    person_1    5000
    person_2    5000
    dtype: int64]
    

""" 
2. Внести данные из таблицы в графовую БД.

Для реализации графового построения необходимо подготовить датафрейм к импорту в среду neo4j:
 - Установить neo4j;
 - Заменить кириллическое представление сущностей в датафрейме на латиницу. 

"""
#pip install neo4j


```python
import unidecode
```


```python
for i in range(len(df)):
    df.loc[i, 'person_1'] = unidecode.unidecode(df['person_1'][i])
    df.loc[i, 'person_2'] = unidecode.unidecode(df['person_2'][i])
df
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>event_id</th>
      <th>person_1</th>
      <th>person_2</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>189</td>
      <td>Galchevskaia Karina Vladimirovna</td>
      <td>Belonovskaia Anastasiia Semenovna</td>
    </tr>
    <tr>
      <th>1</th>
      <td>206</td>
      <td>Ofitserov Oleg Romanovich</td>
      <td>Sapozhnik Boris Valer'evich</td>
    </tr>
    <tr>
      <th>2</th>
      <td>445</td>
      <td>Zhandarova Larisa Germanovna</td>
      <td>Chemodurov Damir Ruslanovich</td>
    </tr>
    <tr>
      <th>3</th>
      <td>503</td>
      <td>Masimova Iana Damirovna</td>
      <td>Mingazhetdinov Ramil' Semenovich</td>
    </tr>
    <tr>
      <th>4</th>
      <td>571</td>
      <td>Mukhtarova Alena Iakovlevna</td>
      <td>Shcherbatenko Ol'ga Robertovna</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>4995</th>
      <td>999333</td>
      <td>Ostashov Vladimir Danilovich</td>
      <td>Chalov Il'ia Vladimirovich</td>
    </tr>
    <tr>
      <th>4996</th>
      <td>999360</td>
      <td>Gandybina Liubov' Aleksandrovna</td>
      <td>Merlin Il'ia Iur'evich</td>
    </tr>
    <tr>
      <th>4997</th>
      <td>999403</td>
      <td>Vostokov Viktor Il'darovich</td>
      <td>Alikas Nikita Andreevich</td>
    </tr>
    <tr>
      <th>4998</th>
      <td>999405</td>
      <td>Ogareva Liudmila Il'darovna</td>
      <td>Nagaitseva Alina Stepanovna</td>
    </tr>
    <tr>
      <th>4999</th>
      <td>999878</td>
      <td>Riapolovskii Georgii Petrovich</td>
      <td>Zhileikin Viktor Pavlovich</td>
    </tr>
  </tbody>
</table>
<p>5000 rows × 3 columns</p>
</div>




```python
"""
Датафрейм был сохранен в новый файл *.csv

"""
df.to_csv('dataset_test.csv', index=False)
```


```python
"""
Для импорта датафрейма в neo4j необходимо импортировать модули GraphDatabase, basic_auth из neo4j. 
Для построения визуального представления импортирован GraphWidget

"""
#pip install yfiles_jupyter_graphs
from neo4j import GraphDatabase, basic_auth
from yfiles_jupyter_graphs import GraphWidget
```


```python
"""
Предварительно в среде Neo4j Desktop была создана пустая база данных. 
Создан драйвер для подключения в среде jupiter

"""
driver = GraphDatabase.driver("bolt://localhost:7687", auth=("neo4j", "12345678"))
```


```python
"""
Непосредтственно импорт в базу происходит по запросу на языке Cypher. 
В этом запросе создаются вершины и связи между вершинами на основе датафрейма. Вершинами являются события и персоны. 
Если вершина с таким же 'Name' была создана ранее, то новая не создается. 

"""
query_import = '''
LOAD CSV FROM 'file:///dataset_test.csv' as line
FIELDTERMINATOR ','
     MERGE (n:event{Name: line[0]})
     MERGE (m:person{Name: line[1]})
     MERGE (s:person{Name: line[2]})
     MERGE (m)-[:R]->(n) 
     MERGE (s)-[:R]->(n)
'''              
```


```python
"""
В этой сессии реализуется запрос 'query_import'. 

"""
session = driver.session()
session.run(query_import)
```




    <neo4j._sync.work.result.Result at 0x21570f38e20>




```python
""" 
Проверка количества уникальных вершин, загруженных в базу. 

"""
query_check = '''
MATCH (n) 
RETURN count(n)
'''
answer = session.run(query_check)
print (answer.single()['count(n)'])
```

    14884
    


```python
"""
Предварительный эвристический анализ датафрейма предполагает наличие сообществ. Необходимо найти эти сообщества. 
Составляется запрос, который "выкидывает" из результата persons и events, которые одновременно удовлетворяют 
следующему условию: person имеет только одно отношение и event который имеет только два отношения. 
Этот запрос выведет вершины и отношения, задействованные в сообществах. Предварительно они помечены как INTER. 
Остальное не представляет интереса для анализа. 
"""
query_label_event = '''
MATCH (n)-[b]-(e:event)
WITH e, count(b) AS con
WHERE con > 2
SET e:INTER
'''
session.run(query_label_event)

query_label_node = '''
MATCH (n)-[b]-(e:event)
WITH n, count(b) AS con
WHERE con > 1
SET n:INTER
'''
session.run(query_label_node)

query_out_INTER = '''
MATCH  (m)-[a]-(e:event)-[b*]-(x)
WHERE m:INTER OR e:INTER
RETURN (m), (e), (x), (a), (b)
'''

graph = session.run(query_out_INTER).graph()
w = GraphWidget(graph = graph)
```


```python
"""
Словарь с визуальными свойствами графа.

"""
styles = {
    'event':{'color': '#6C7400', 'shape': 'ellipse', 'label':'Name'},
    'person':{'color': '#005977', 'shape': 'octagon', 'label':'Name'}
}
```


```python
"""
"Наделение" свойствами визуализиемого графа

"""
w.set_node_styles_mapping(lambda index, node: styles.get(node['properties']['label'], {}))
w.set_node_label_mapping(lambda index, node: node['properties']['Name'])
```


```python
len(w.nodes)
```




    280




```python
len(w.edges)
```




    264




```python
w.show()
```


    GraphWidget(layout=Layout(height='500px', width='100%'))


Визуально было обнаружено 20 сообществ. Под сообществом понимается связанная структура из более чем одного события или более чем двух персон. В некоторых сообществах есть персоны - лидеры, однако большинство сообществ небольшие и центральным объектом таких небольших сообществ является событие. На следующем этапе анализа из графового представления выделены только минимальные сообщества. Алгоритм их выделения показан в запросе query_min_social


```python
'''
Выглядят маленькие сообщества одинаково. Визуальный пример одного такого маленького сообщества.

'''
query_min_social_example = '''
MATCH  (e:event {Name: '70049'})<-[m]-(x)
RETURN  e, x, m 
'''
session = driver.session()
graph = session.run(query_min_social_example).graph()
xmp = GraphWidget(graph = graph)
xmp.set_node_label_mapping(lambda index, node: node['properties']['Name'])
xmp.show()
```


    GraphWidget(layout=Layout(height='500px', width='100%'))



```python
query_min_social = '''
MATCH  (e:INTER)<-[*]-(x)
WITH e.Name AS Name, collect (x) AS prt
RETURN  Name, prt 
'''
session = driver.session()
min_social = session.run(query_min_social)

'''
Вывод списка с номером минимального сообщества и его участниками. 
Номер сообщества - это номер события, "вокруг" которого собрались участники.
Это маленькие сообщества без лидера. Стоит отметить, что сообщество 'Name': '850472'
попало с список минимальных, но также оно является частью большого сообщества. 
'''  
min_social.data()
```




    [{'Name': '70049',
      'prt': [{'Name': 'Fedova Anzhelika Vadimovna'},
       {'Name': "Iashina Polina Evgen'evna"},
       {'Name': "Val'dovskii Al'bert Efimovich"},
       {'Name': 'Gerasimovskaia Kseniia Damirovna'}]},
     {'Name': '92995',
      'prt': [{'Name': "Kucherenko Irina Il'inichna"},
       {'Name': "Bad'ianova Rimma Maksimovna"},
       {'Name': 'Boltik Grigorii Maksimovich'},
       {'Name': "Zhurik Al'bert Evgen'evich"}]},
     {'Name': '117280',
      'prt': [{'Name': "Utochkin Evgenii Anatol'evich"},
       {'Name': 'Kaganovich Liliia Petrovna'},
       {'Name': 'Gaisumov Viktor Timurovich'},
       {'Name': 'Volynskii Kirill Fedorovich'}]},
     {'Name': '177407',
      'prt': [{'Name': 'Zelinskii Gennadii Arturovich'},
       {'Name': 'Buzhaninov Ruslan Arturovich'},
       {'Name': 'Sorokovoi German Maratovich'},
       {'Name': "Zazorin Vadim Arkad'evich"}]},
     {'Name': '358194',
      'prt': [{'Name': 'Serpukhova Alla Iaroslavovna'},
       {'Name': "Brusentsova Dar'ia Mikhailovna"},
       {'Name': 'Dolgikh Liliia Vadimovna'},
       {'Name': 'Noeva Galina Stepanovna'}]},
     {'Name': '390312',
      'prt': [{'Name': "Okhotsimskaia Viktoriia Evgen'evna"},
       {'Name': 'Khilin Fedor Fedorovich'},
       {'Name': 'Namazova Evgeniia Dmitrievna'},
       {'Name': "Grigor'evykh Pavel Leonidovich"}]},
     {'Name': '523688',
      'prt': [{'Name': 'Batskikh Egor Olegovich'},
       {'Name': 'Botianovskaia Antonina Danilovna'},
       {'Name': 'Pamfilova Tamara Danilovna'},
       {'Name': 'Barilov Roman Filippovich'}]},
     {'Name': '551592',
      'prt': [{'Name': "Solonchenko Karina Vasil'evna"},
       {'Name': 'Nugumanov Efim Andreevich'},
       {'Name': "Zakhar'eva Irina Denisovna"},
       {'Name': 'Vybornov Dmitrii Dmitrievich'}]},
     {'Name': '613539',
      'prt': [{'Name': 'Gavrilenko Gleb Marselevich'},
       {'Name': "Sakhnova Tamara Vasil'evna"},
       {'Name': "Ganenko El'mira Stepanovna"},
       {'Name': 'Kishenin Stanislav Georgievich'}]},
     {'Name': '716489',
      'prt': [{'Name': 'Arsenchuk Ruslan Denisovich'},
       {'Name': 'Shtin Maksim Ruslanovich'},
       {'Name': "Adel'khanova Elena Petrovna"},
       {'Name': 'Panteliukhina Larisa Viacheslavovna'}]},
     {'Name': '765223',
      'prt': [{'Name': "Savluk Marsel' Vladimirovich"},
       {'Name': 'Babosov Mikhail Konstantinovich'},
       {'Name': "Guleva Marina Vital'evna"},
       {'Name': 'Atamkulova Mariia Andreevna'}]},
     {'Name': '850472',
      'prt': [{'Name': 'Borchin Pavel Robertovich'},
       {'Name': 'Akhromeeva Alina Ivanovna'},
       {'Name': "Larishchev Il'ia Aleksandrovich"},
       {'Name': 'Strik Elina Marselevna'}]},
     {'Name': '873359',
      'prt': [{'Name': "Iakimikhina Natal'ia Ianovna"},
       {'Name': 'Starovoitov Viacheslav Pavlovich'},
       {'Name': "Ulissov Marsel' Eduardovich"},
       {'Name': "Dzhanibekov Nikita Iur'evich"}]},
     {'Name': '938764',
      'prt': [{'Name': "Khrisogonov Ivan Gennad'evich"},
       {'Name': 'Samolov Mikhail Alekseevich'},
       {'Name': 'Dvigubskaia Iana Ivanovna'},
       {'Name': 'Soltaganov Fedor Efimovich'}]},
     {'Name': '985851',
      'prt': [{'Name': "Pavliukova Natal'ia Fedorovna"},
       {'Name': "Kleban Igor' Glebovich"},
       {'Name': 'Lipunova Galina Rinatovna'},
       {'Name': "Notkina Al'bina Mikhailovna"}]}]



Далее необходимо рассмотреть большие сложные сообщества 


```python
"""Визуализация только сложных сообществ"""
query_max_social_graph = '''
MATCH  (m:INTER)-[b*]-(e:event)-[a*]-(x:person)
RETURN *
'''
graph = session.run(query_max_social_graph).graph()
s_max = GraphWidget(graph = graph)
s_max.set_node_styles_mapping(lambda index, node: styles.get(node['properties']['label'], {}))
s_max.set_node_label_mapping(lambda index, node: node['properties']['Name'])
s_max.show()
```


    GraphWidget(layout=Layout(height='500px', width='100%'))


Визуально можно выделить 6 сложных сообществ. Их перечень представлен ниже.
Внутри этих сообществ следует выделить лидера. Под лидерами понимаются персоны, 
обладающие более чем одной связью с событиями и другими персонами.  
Далее рассмотрено каждое сообщество отдельно. В сложных сообществах может не быть явного лидера и для полноценного
анализа стоит учитывать контекст данных, чтобы определить заранее перечень ролей для отдельных персон. 


```python
'''
Список ключевых участников каждого из шести сообществ. Эти персоны были помечены как имеющие более одной связи.
Далее каждое из сложных сообществ будет проанализировано отдельно и для его построения будет применено имя 
персоны из списка. 
'''
query_list_lead = '''
MATCH  (m:INTER)-[a]->(e:event)-[*]-(x)
RETURN DISTINCT (m) AS lead
'''
leads = session.run(query_list_lead).data()
lead_list = []
for i in leads: 
    Name_lead = i.values()
    Name_lead = list(Name_lead)[0].values()
    lead_list.append(list(Name_lead))
lead_list
```




    [['Akhromeeva Alina Ivanovna'],
     ['Bashnina Antonina Glebovna'],
     ['Pafomova Kira Vadimovna'],
     ['Zimnukhova Karina Danilovna'],
     ["Medvedeva Dar'ia Alekseevna"],
     ['Nedoveskov Vladimir Ivanovich'],
     ["Sholokhov Igor' Robertovich"],
     ['Danilenko Vladimir Semenovich'],
     ["Mailina Gul'nara Ivanovna"],
     ['Anikhnova Tamara Ruslanovna'],
     ["Dvigubskaia Valentina Gennad'evna"],
     ["Diomidov Igor' Il'darovich"],
     ['Ivashev Viacheslav Igorevich'],
     ['Nagaitseva Anzhelika Ianovna'],
     ['Radionova Tamara Iaroslavovna'],
     ['Batievskaia Angelina Romanovna'],
     ["Iatskoi Robert Il'darovich"],
     ['Troekurov Gleb Efimovich'],
     ["Kaekhtin Il'dar Eduardovich"],
     ['Torgunakov Roman Kirillovich'],
     ['Dorozhkin Anatolii Egorovich'],
     ['Podolian Vladislav Denisovich'],
     ['Poskrebyshev Iakov Dmitrievich'],
     ["Ryskina El'mira Ivanovna"],
     ['Liaudanskii Valentin Vladislavovich'],
     ["Marakhovskaia Dar'ia Romanovna"],
     ['Bugaichuk Roman Eduardovich']]



Визуализация и состав первого сообщества: 


```python
query_social_1 = '''
MATCH  (m:INTER {Name:'Akhromeeva Alina Ivanovna'})-[b]->(e:event)-[a*]-(x) 
RETURN *
'''
session = driver.session()
graph = session.run(query_social_1).graph()
s1 = GraphWidget(graph = graph)
s1.set_node_styles_mapping(lambda index, node: styles.get(node['properties']['label'], {}))
s1.set_node_label_mapping(lambda index, node: node['properties']['Name'])
s1.show()
```


    GraphWidget(layout=Layout(height='500px', width='100%'))



```python
query_social_1_list = '''MATCH (m:INTER{Name:'Akhromeeva Alina Ivanovna'})-[*]-(e:event)-[a*]-(x)
WITH m AS lead, count(x) AS degree, collect(x) AS partiсipant
RETURN lead, degree, partiсipant'''

participant_1= session.run(query_social_1_list).data()
participant_1
```




    [{'lead': {'Name': 'Akhromeeva Alina Ivanovna'},
      'degree': 52,
      'partiсipant': [{'Name': "Nepomniashchikh Il'ia Damirovich"},
       {'Name': 'Netuzhilova Elena Viktorovna'},
       {'Name': 'Borgolov Evgenii Maratovich'},
       {'Name': 'Shchurupova Alla Filippovna'},
       {'Name': 'Chikireva Mariia Romanovna'},
       {'Name': "Bugaenkova Karina Arkad'evna"},
       {'Name': "Abarenov Il'dar Robertovich"},
       {'Name': 'Solomeina Kristina Georgievna'},
       {'Name': 'Salagaev Ivan Ramilevich'},
       {'Name': 'Bekreva Viktoriia Iakovlevna'},
       {'Name': 'Kamilov Damir Pavlovich'},
       {'Name': "Bordachev Nikita Vasil'evich"},
       {'Name': "Sarsadskikh Alena Gennad'evna"},
       {'Name': "Akodes Efim Anatol'evich"},
       {'Name': "Chechin Ramil' Konstantinovich"},
       {'Name': "Shovkovskaia Natal'ia Nikolaevna"},
       {'Name': "Shchennikov Dmitrii Grigor'evich"},
       {'Name': 'Soloveichikov Oleg Pavlovich'},
       {'Name': 'Kamchadalov Artem Iaroslavovich'},
       {'Name': 'Urmantseva Evgeniia Olegovna'},
       {'Name': 'Aidamirova Karina Antonovna'},
       {'Name': 'Blizniakov Ivan Artemovich'},
       {'Name': 'Domogarov Anton Maksimovich'},
       {'Name': 'Belogorlov Damir Kirillovich'},
       {'Name': 'Arbachakov Filipp Andreevich'},
       {'Name': 'Alipichev Evgenii Timurovich'},
       {'Name': 'Tolkunova Valentina Maratovna'},
       {'Name': 'Zhubanov Anatolii Ivanovich'},
       {'Name': 'Borchin Pavel Robertovich'},
       {'Name': "Larishchev Il'ia Aleksandrovich"},
       {'Name': 'Strik Elina Marselevna'},
       {'Name': 'Guzhov Gleb Danilovich'},
       {'Name': 'Starukhin Damir Maratovich'},
       {'Name': 'Rasulev Nikita Petrovich'},
       {'Name': 'Tiazhlov Rinat Vladislavovich'},
       {'Name': "Oshurov Pavel Il'darovich"},
       {'Name': 'Kutasov Konstantin Sergeevich'},
       {'Name': 'Dudykina Mariia Romanovna'},
       {'Name': 'Dumler Liudmila Viacheslavovna'},
       {'Name': 'Kovshov Gleb Germanovich'},
       {'Name': 'Andrievskaia Marina Rinatovna'},
       {'Name': "Preobrazhenskaia Kira Al'bertovna"},
       {'Name': "Shal'nova Ol'ga Vladimirovna"},
       {'Name': 'Bobretsova Svetlana Artemovna'},
       {'Name': "Selin Fedor Il'ich"},
       {'Name': 'Ianikeev Viacheslav Ruslanovich'},
       {'Name': "Saidenov Ivan Valer'evich"},
       {'Name': 'Muzalevskaia Angelina Fedorovna'},
       {'Name': "Iashchukova Liubov' Efimovna"},
       {'Name': "Aksanova Kristina Grigor'evna"},
       {'Name': 'Bodriakova Evgeniia Ianovna'},
       {'Name': 'Vokhmentsev Vladimir Vladislavovich'}]}]




```python
query_social_2 = '''
MATCH  (m:INTER {Name:'Bashnina Antonina Glebovna'})-[b]->(e:event)-[a*]-(x) 
RETURN *
'''
session = driver.session()
graph = session.run(query_social_2).graph()
s2 = GraphWidget(graph = graph)
s2.set_node_label_mapping(lambda index, node: node['properties']['Name'])
s2.set_node_styles_mapping(lambda index, node: styles.get(node['properties']['label'], {}))
s2.show()
```


    GraphWidget(layout=Layout(height='500px', width='100%'))



```python
query_social_2_list = '''MATCH (m:INTER{Name:'Bashnina Antonina Glebovna'})-[*]-(e:event)-[a*]-(x)
WITH m AS lead, count(x) AS degree, collect(x) AS partiсipant
RETURN lead, degree, partiсipant'''

participant_2= session.run(query_social_2_list).data()
participant_2
```




    [{'lead': {'Name': 'Bashnina Antonina Glebovna'},
      'degree': 14,
      'partiсipant': [{'Name': 'Sergulin Egor Glebovich'},
       {'Name': "Akramova Liudmila Al'bertovna"},
       {'Name': 'Zhandarev Viktor Glebovich'},
       {'Name': 'Kuporov Grigorii Stanislavovich'},
       {'Name': "Argentovskaia Evgeniia Evgen'evna"},
       {'Name': "Chelombit'ko Timofei Antonovich"},
       {'Name': 'Susaikin Vladimir Iaroslavovich'},
       {'Name': 'Nedokvasov Vladislav Konstantinovich'},
       {'Name': 'Golovushina Alla Filippovna'},
       {'Name': 'Pavliuchikov Maksim Filippovich'},
       {'Name': 'Gabov Boris Marselevich'},
       {'Name': 'Fefilov Dmitrii Ianovich'},
       {'Name': 'Aliabysheva Diana Iaroslavovna'},
       {'Name': 'Kazak Nikolai Ruslanovich'}]}]




```python
query_social_3 = '''
MATCH  (m:INTER {Name:'Bugaichuk Roman Eduardovich'})-[b]-(e:event)-[a*]-(x) 
RETURN *
'''
session = driver.session()
graph = session.run(query_social_3).graph()
s3 = GraphWidget(graph = graph)
s3.set_node_styles_mapping(lambda index, node: styles.get(node['properties']['label'], {}))
s3.set_node_label_mapping(lambda index, node: node['properties']['Name'])
s3.show()
```


    GraphWidget(layout=Layout(height='500px', width='100%'))


Это сообщество интересное. Явного лидера не представляется возможным выделить, однако запрос для построения выполнен
относительно централной персоны. Относительно нее будут высчитаны степени связности. Возможно также ввести понятие "веса" персоны. Введена колонка степени связности для членов такого сообщества.


```python
query_social_3_list = '''
MATCH p = (m:INTER{Name: 'Bugaichuk Roman Eduardovich'})-[z*]-(x:person) 
RETURN  m AS start_member, collect(x) AS participant, length(p)/2 AS step
'''
participant_3 = session.run(query_social_3_list).data()
participant_3
```




    [{'start_member': {'Name': 'Bugaichuk Roman Eduardovich'},
      'participant': [{'Name': 'Radionova Tamara Iaroslavovna'},
       {'Name': 'Anikhnova Tamara Ruslanovna'}],
      'step': 1},
     {'start_member': {'Name': 'Bugaichuk Roman Eduardovich'},
      'participant': [{'Name': "Diomidov Igor' Il'darovich"},
       {'Name': 'Zimnukhova Karina Danilovna'}],
      'step': 2},
     {'start_member': {'Name': 'Bugaichuk Roman Eduardovich'},
      'participant': [{'Name': 'Pitenin Iaroslav Radikovich'},
       {'Name': 'Karsanov Damir Timurovich'},
       {'Name': "Tiktinskaia Galina Grigor'evna"},
       {'Name': "Starobinskaia Iana Al'bertovna"},
       {'Name': 'Loviagin Dmitrii Denisovich'},
       {'Name': "Lanchikova Al'bina Valer'evna"},
       {'Name': 'Lekanova Tamara Ivanovna'},
       {'Name': 'Krivoviazova Kseniia Iaroslavovna'}],
      'step': 3}]



Такое представление в совокупности с визуальным дает информацию о структуре и составе сообщества


```python
query_social_4 = '''
MATCH  (m:INTER {Name:"Medvedeva Dar'ia Alekseevna"})-[b]->(e:event)-[a*]-(x) 
RETURN *
'''
session = driver.session()
graph = session.run(query_social_4).graph()
s4 = GraphWidget(graph = graph)
s4.set_node_styles_mapping(lambda index, node: styles.get(node['properties']['label'], {}))
s4.set_node_label_mapping(lambda index, node: node['properties']['Name'])
s4.show()
```


    GraphWidget(layout=Layout(height='500px', width='100%'))


Это несложное сообщество с явным лидером и равноудаленными от него участниками


```python
query_social_4_list = '''
MATCH (m:INTER{Name:"Medvedeva Dar'ia Alekseevna"})-[*]-(e:event)-[a]-(x)
WITH m AS lead, count(x) AS degree, collect(x) AS partisipant
RETURN lead, degree, partisipant
'''
participant_4 = session.run(query_social_4_list).data()
participant_4
```




    [{'lead': {'Name': "Medvedeva Dar'ia Alekseevna"},
      'degree': 6,
      'partisipant': [{'Name': "Kondrat'ev Boris Germanovich"},
       {'Name': 'Pomykalova Tamara Fedorovna'},
       {'Name': 'Glazkov Artur Petrovich'},
       {'Name': 'Bezgachii Denis Efimovich'},
       {'Name': "Dubrova Anzhelika Grigor'evna"},
       {'Name': 'Pchelintsev Artur Glebovich'}]}]




```python
query_social_5 = '''
MATCH  (m:INTER {Name:"Nedoveskov Vladimir Ivanovich"})-[b]->(e:event)-[a*]-(x) 
RETURN *
'''
session = driver.session()
graph = session.run(query_social_5).graph()
s5 = GraphWidget(graph = graph)
s5.set_node_styles_mapping(lambda index, node: styles.get(node['properties']['label'], {}))
s5.set_node_label_mapping(lambda index, node: node['properties']['Name'])
s5.show()
```


    GraphWidget(layout=Layout(height='500px', width='100%'))


Круговое сообщество. В качестве характеристик имеет смысл перечислить участников и обозначить количество шагов от стартовой вершины до каждой. Алгоритм запроса обойдет граф с двух сторон и покажет число шагов до каждого участника с каждой стороны. Лидера нет. 


```python
query_social_5_list = '''
MATCH p =  (m:INTER{Name: 'Nedoveskov Vladimir Ivanovich'})-[z*]-(x:person) 
RETURN  m AS start_member, collect(x) AS participant, length(p)/2 AS step
'''
participant_5 = session.run(query_social_5_list).data()
participant_5
```




    [{'start_member': {'Name': 'Nedoveskov Vladimir Ivanovich'},
      'participant': [{'Name': "Iatskoi Robert Il'darovich"},
       {'Name': 'Podolian Vladislav Denisovich'}],
      'step': 1},
     {'start_member': {'Name': 'Nedoveskov Vladimir Ivanovich'},
      'participant': [{'Name': "Kaekhtin Il'dar Eduardovich"},
       {'Name': 'Poskrebyshev Iakov Dmitrievich'}],
      'step': 2},
     {'start_member': {'Name': 'Nedoveskov Vladimir Ivanovich'},
      'participant': [{'Name': "Mailina Gul'nara Ivanovna"},
       {'Name': "Mailina Gul'nara Ivanovna"}],
      'step': 3},
     {'start_member': {'Name': 'Nedoveskov Vladimir Ivanovich'},
      'participant': [{'Name': 'Poskrebyshev Iakov Dmitrievich'},
       {'Name': "Kaekhtin Il'dar Eduardovich"}],
      'step': 4},
     {'start_member': {'Name': 'Nedoveskov Vladimir Ivanovich'},
      'participant': [{'Name': 'Podolian Vladislav Denisovich'},
       {'Name': "Iatskoi Robert Il'darovich"}],
      'step': 5},
     {'start_member': {'Name': 'Nedoveskov Vladimir Ivanovich'},
      'participant': [{'Name': 'Nedoveskov Vladimir Ivanovich'},
       {'Name': 'Nedoveskov Vladimir Ivanovich'}],
      'step': 6}]




```python
query_social_6 = '''
MATCH  (m:INTER {Name:"Sholokhov Igor' Robertovich"})-[b]->(e:event)-[a*]-(x) 
RETURN *
'''
session = driver.session()
graph = session.run(query_social_6).graph()
s6 = GraphWidget(graph = graph)
s6.set_node_styles_mapping(lambda index, node: styles.get(node['properties']['label'], {}))
s6.set_node_label_mapping(lambda index, node: node['properties']['Name'])
s6.show()
```


    GraphWidget(layout=Layout(height='500px', width='100%'))



```python
query_social_6_list ='''
MATCH p = (m:INTER {Name:"Sholokhov Igor' Robertovich"})-[a*]-(prt:person) 
RETURN min(length(p)/2) AS path, prt, count(a) AS con
'''
participant_6 = session.run(query_social_6_list).data()
participant_6
```




    [{'path': 1, 'prt': {'Name': 'Nagaitseva Anzhelika Ianovna'}, 'con': 5},
     {'path': 2, 'prt': {'Name': 'Troekurov Gleb Efimovich'}, 'con': 5},
     {'path': 2, 'prt': {'Name': "Marakhovskaia Dar'ia Romanovna"}, 'con': 5},
     {'path': 1, 'prt': {'Name': 'Pafomova Kira Vadimovna'}, 'con': 9},
     {'path': 4, 'prt': {'Name': "Sholokhov Igor' Robertovich"}, 'con': 6},
     {'path': 1, 'prt': {'Name': "Dvigubskaia Valentina Gennad'evna"}, 'con': 21},
     {'path': 2, 'prt': {'Name': 'Ivashev Viacheslav Igorevich'}, 'con': 14},
     {'path': 3, 'prt': {'Name': "Ryskina El'mira Ivanovna"}, 'con': 14},
     {'path': 4, 'prt': {'Name': 'Danilenko Vladimir Semenovich'}, 'con': 14},
     {'path': 3, 'prt': {'Name': 'Batievskaia Angelina Romanovna'}, 'con': 14},
     {'path': 2, 'prt': {'Name': 'Dorozhkin Anatolii Egorovich'}, 'con': 14},
     {'path': 1, 'prt': {'Name': 'Torgunakov Roman Kirillovich'}, 'con': 5},
     {'path': 2, 'prt': {'Name': 'Liaudanskii Valentin Vladislavovich'}, 'con': 5}]



Самое непростое для анализа сообщество. За стартовую персону был взят "Sholokhov Igor' Robertovich". Посчитан минимальный путь обхода сообщества до каждого из участников path. Значение con характеризует количество возможных путей между стартовой персоной и каждым участником.

Таким образом удалось следующее: 
    - Загрузить датафрейм из файла CSV для подготовки к импорту в neo4j;
    - Импортировать данные в базу Neo4j с корректным учетом связей между сущностями;
    - Наладить связь из среды jupyter с базой Neo4j;
    - Составить несколько запросов, позволяющих провести поверхностный анализ сообществ и вывести результаты анализа: визуальные и количественные;
Не удалось: 
    - Применить алгоритмы автоматического поиска сообществ в базе по определенным признакам (кроме признака "по количеству связей");
    - Применить форматирование строк для управления содержанием запроса на языке Cypher через средтва Python;
