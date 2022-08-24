# Многослойный персептрон (Sloan Digital Sky Survey DR16)
Курсовая работа по теме "Построение многослойной нейронной сети без обратных связей на нейронах с нелинейной функцией активации"

## Предварительный анализ
В качестве данных для обучения и демонстрации работы сети была выбрана база
данных с сайта kaggle.com: [Sloan Digital Sky Survey DR16](https://github.com/ArinaOwl/multilayer_perceptron/blob/main/data/Skyserver_12_30_2019%204_49_58%20PM.csv).

Таблица содержит 18 столбцов (целевые данные и 17 признаков). Целью работы сети является определение класса небесного объекта. Целевая переменная (`class`) представлена 3 классами: 
- галактики (GALAXY), 
- звезды (STAR), 
- квазары (QSO). 

Так как это единственные данные типа object, нужно закодировать только их, а также, так как эта переменная является целевой, можно воспользоваться обычным кодированием целыми числами (label encoding).

Пердварительный анализ 17 признаков показал, что можно исключить:
- `rerun`, т.к. содержит только одно значение;
- `ra` (прямое восхождение), `dec` (склонение) и `mjd` (дата наблюдения), т.к. в совокупности характеризуют исследуемый сектор небосклона в дату наблюдения и не имеют отношения к типу наблюдаемого объекта;
- `run` (номер наблюдения), `camcol` (номер колонки камеры), `field` (номер поля камеры) и `plate` (номер пластины), т.к. не имеют влияния на тип наблюдаемого объект;
- `objid`, `specobjid`, `fiberid`, т.к. это идентификационные переменные.

Корреляция между выбранными признаками и закодированной целевой переменной `classid`:
- между `classid` и `redshift` достаточно сильная,
- между `classid` и другими характеристиками слабее, однако, они могут дополнить информацию об объекте, чтобы с достаточной точностью определить его класс,
- сильная между смежными признаками объясняется тем, что данные получены в смежных диапазонах спектра.

![corr](https://github.com/ArinaOwl/multilayer_perceptron/blob/main/images/corr.png)

Графики проекций объектов в многомерном пространстве характеристик на плоскости, образуемые парами этих характеристик позволяют сделать вывод, что объекты образуют кластеры в многомерном пространстве и принципиально возможна классификация объектов по выбранному набору характеристик.

![scatter_matrix](https://github.com/ArinaOwl/multilayer_perceptron/blob/main/images/scatter_matrix.png)

## Анализ зависимости качества модели от гиперпараметров

Исходная таблица с данными была поделена на три части: обучающую (train) 7000 строк, проверочную (validation) 3000 строк и тестовую (test) 90000 строк.

Для сети, состоящей из трех скрытых слоев по 50 нейронов с функцией активации ReLU, за 20-30 эпох:
- потери уменьшаются почти в 10 раз и выходят на насыщение, 
- точность выходит на уровень 95-97% правильно классифицированных объектов.

| Loss | Accuracy |
| --- | --- |
| ![loss](https://github.com/ArinaOwl/multilayer_perceptron/blob/main/images/loss.png) | ![acc](https://github.com/ArinaOwl/multilayer_perceptron/blob/main/images/acc.png) |

Были проведены исследования влияния на значения функции потерь: 
- количества скрытых слоев нейронной сети, 
- количества нейронов в одном слое, 
- величины аргумента `p` функции отсеивания нейронов в слое `Dropout(p)`.

| Количество скрытых слоев |
| ![num_hid](https://github.com/ArinaOwl/multilayer_perceptron/blob/main/images/num_hid.png) |  |
| ![num_hid_min_loss](https://github.com/ArinaOwl/multilayer_perceptron/blob/main/images/num_hid_min_loss.png) |  |
