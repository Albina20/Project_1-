В проекте были задействованы следующие библиотеки:
pandas  
datetime as dt  
calendar  
seaborn as sns  
matplotlib.pyplot  
numpy    
pingouin  
scipy.stats   
tqdm  

### Задание
В крупном дейтинговом приложении, помимо базовых функций, также имеется премиум-подписка.

В ходе A/B тестирования некоторой гипотезы целевой группе была предложена новая стоимость премиум-подписки для новых пользователей из нескольких стран при покупке через две новые платежные системы. У двух контрольных групп условия премиум-подписки не изменялись.

Необходимо проанализировать итоги эксперимента и сделать вывод, стоит ли запускать новую стоимость услуги на всех пользователей.

    
### ПЛАН РАБОТЫ:
1.	Определение метрик для проверки успешности эксперимента
2.	Загрузка данных, проверка данных в датасете, предобработка данных
3.	Проверка выборок на репрезентативность
4.	А/А тестирование. Проверка системы сплитования
5.	А/В тестирование
6.	Вывод: был ли эксперимент успешен.

### 1.	Определение метрик
Так как целевой группе была предложена новая стоимость услуги на сайте, то в первую очередь это повлияет на конверсию пользователя в премиум пользователя (CR). Именно эту метрику возьмем за основу для выводов.
Сопутствующие метрики, которые можно отследить, чтобы они не падали: средняя выручка всех пользователей сайта(ARPU), средняя выручка премиум пользователей (ARPPU).

### 2.	Загрузка данных, проверка данных в датасете, предобработка данных
В результате предобработки сформирован датасет, в котором:
-	данные загружены, прочитаны, проверены типы данных, наличие дубликатов.
-	определены три группы пользователей (тестовая и две контрольных) с регистрацией в период эксперимента; 
-	отобраны данные из тех стран, где проводилось исследование (страны в контрольных группах совпадают с тестовой); 
-	каждому пользователю добавлена сумма его трат в первую очередь на подписку Премиум, на пробную подписку и на остальные продукты.

### 3.	Проверка выборок на репрезентативность
Посмотрим на количество пользователей. Почти одинаковое количество во всех группах: 
![image](https://github.com/Albina20/Project_1/assets/59622108/fb55d45f-d51c-4649-afb9-ea9303c81927)

Распределение по гендеру одинаково во всех трех группах:
![image](https://github.com/Albina20/Project_1/assets/59622108/5e66143f-af38-4129-ac61-303b5cd946c0)

На диаграмме видно, что данные между группами пользователей по странам распределены равномерно. USA значительно выделяется по количеству пользователей:
![image](https://github.com/Albina20/Project_1/assets/59622108/60a910b3-b75c-4f26-934d-b3c41c566ea9)

Также была проведена проверка на репрезентативность по коэффициенту привлекательности, границам поиска по возрасту.
Диаграммы показали, что выборки репрезентативны по признакам, не связанными с основными метриками. По гендерному признаку, по странам, по возрасту, по коэффициенту привлекательности, границам поиска по возрасту группы примерно равнозначны.  
Таким образом, можно сказать, что данные в группах распределены равномерно. A/A тестирование должно дать позитивный результат.

### 4.	А/А тестирование. Проверка системы сплитования
До проведения A/B тестов нужно провести А/A тестирование, которое должно подтвердить однородность групп и корректность системы сплитования.  
Проверим метрики, которые мы выбрали для интерпритации результатов А/В тестирования:
-	средняя выручка всех пользователей сайта(ARPU)
-	средняя выручка премиум пользователей (ARPPU)
-	конверсия пользователя в премиум пользователя (CR)

Сначала проведём сравнение двух контрольных групп.   
Количество пользователей в контрольных группах почти одинаково:
![Количество в группах](https://github.com/Albina20/Project_1/assets/59622108/3878b9b6-e9fd-4393-9fce-0e9a29580cca)

Сравним среднюю выручку от всех продуктов по всем пользователям в контрольных группах (ARPU_all):
![image](https://github.com/Albina20/Project_1/assets/59622108/e06b45c3-8b93-4659-9da3-973bd076af0b)

Проверю данные на различие в дисперсиях  
•	H0 - статистически значимого различия в дисперсиях нет  
•	Н1 - статистически значимое различие в дисперсиях есть
![Левене 1](https://github.com/Albina20/Project_1/assets/59622108/f1a7b780-9df8-49c0-9d71-60d70fef0cf3)

pval > 0.05, equal_var=True   
=> принимаем H0 - статистически значимого различия в дисперсиях нет.

Сравнение средних значений выручки всех пользователей используя ttest получились следующим:   
__Ttest_indResult(statistic=0.9417397553720265, pvalue=0.34635241889072266)__  
pvalue > 0.05  
Принимаем H0 - статистически значимого различия в дисперсиях нет.   
Значит средняя выручка всех пользователей в контрольных группах статистически значимо не отличается.

Сравниваю среднюю выручку премиум пользователей в контрольных группах (ARPPU)
![image](https://github.com/Albina20/Project_1/assets/59622108/80804325-cbc1-4f10-bcdb-a7d3993bef81)

Тест Левене:
![image](https://github.com/Albina20/Project_1/assets/59622108/9948e8f7-c65f-4f9a-a3ae-8338112a7f70)
pval > 0.05, equal_var = True  
принимаем H0 - статистически значимого различия в дисперсиях нет.  

Сравню средние значения выручки Премиум пользователей используя ttest
__Ttest_indResult(statistic=0.3710159116724573, pvalue=0.7110223402654123)__  
pvalue > 0.05  
Принимаем H0 - статистически значимого различия в дисперсиях нет.     
Значит средняя выручка Премиум пользователей в контрольных группах статистически значимо не отличается.

#### CR
Для проверки статистичестически взаимосвязи между наличием премиум и распределением по группам выбираю хи-квадрат, потому что сравниваю качественные метрики.  
-	Н0 взаимосвязи между переменными нет, наличие премиум подписки не зависит от распределения по группам, между группами по этому признаку различий нет.  
-	Н1 взаимосвязь между переменными есть, наличие премиум подписки зависит от распределения по группам, между группами есть различие по этому признаку

__Результат Хи квадрат, pval > 0.05__  

Принимаем Н0: взаимосвязи между переменными нет, наличие премиум подписки не зависит от распределения по группам, между группами по этому признаку различий нет.  
Рассчитываем CR и проверяю верность расчетов с помощью формулы ARPU = ARPPU * CR. Проверка пройдена успешно. Метрики рассчитаны верно.  
Результаты A/A тестирования подтвердили, что система сплитования работает правильно, переходим к А/В тестированию

### 5.	А/В тестирование
Чтобы понять насколько статистически значимо различаются конверсии - проведем A/B тест.  
-	Нулевая гипотеза (H0): Разницы между конверсией пользователя в премиум пользователя в тестовой и контрольной группе нет.  
-	Альтернативная гипотеза (H1): Разница между конверсией пользователя в премиум пользователя в тестовой и контрольной группе есть.

Поскольку данные в полях "покупка (переход пользователя в премиум пользователя) " и "группа" являются категориальными переменными, то будем использовать тест Хи-квадрат.  
#### Тест Хи-квадрат
Требования: наблюдения не зависимы (выполняется); в каждой ячейке более 5 значений (выполняется).
-	Н0: взаимосвязи между переменными нет, наличие премиум подписки не зависит от распределения по группам, между группами по этому признаку различий нет
-	Н1: взаимосвязь между переменными есть, наличие премиум подписки зависит от распределения по группам, между группами есть различие по этому признаку.

 Результаты статистики
chi2, p, dof, expected = chi2_contingency(tab)
p = 0.016557101788717197
Хи квадрат, pval < 0.05 

Итого:
Принимаем Н1 : взаимосвязь между переменными есть, наличие премиум подписки зависит от распределения по группам, между группами есть различие по этому признаку.  

Анализ сопутствующих метрик
Были рассчитаны метрики ARPU и ARPPU для тестовой и контрольной групп. Проведено сравнение средних используя тест Левене и ttest. Статистически значимого различия в дисперсиях нет. Значит средняя выручка в контрольной и тестовых группах статистически значимо не отличается.

### 6.	Вывод: был ли эксперимент успешен.
- Конверсия в покупку: применяя тест Хи-квадрат мы не смогли отклонить H0 об отсутствии взаимосвязи между группой и покупкой премиум статуса пользователем.
- Средняя выручка всех пользователей сайта(ARPU), средняя выручка премиум пользователей (ARPPU). Применяя Тест Левена и ttest мы обнаружили, что статистически значимых различий нет.

После изменения стоимости премиум-подписки для новых пользователей из нескольких стран:
1) Средняя выручка с пользователей платформы онлайн-знакомств статистически значимо НЕ изменилась.
2) Средняя выручка с платящих пользователей статически значимо НЕ изменилась.
3) Наблюдаемое повышение показателей средней выручки в тестовой группе (ARPU , ARPPU) произошло скорее всего из-за повышения цены Премиум подписки.
4) Доля пользователей платформы онлайн-знакомств, приобретающих платную Премиум подписку статистически значимо снизилась.

В целом по исследованию мы видим снижение конверсии, при средней неизменной прибыли.    
#### ВЫВОД: эксперимент НЕ успешен, при изменении стоимости премиум подписки конверсия падает, пользователи НЕ хотят приобретать премиум подписку по новой цене.


