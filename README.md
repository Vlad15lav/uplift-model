# Uplift моделирование

Uplift моделирование представляет собой систему, разработанную для анализа и прогнозирования эффекта воздействия на целевую аудиторию. Данный кейс был выполнен благодаря материалам X5 Retail Group и тестового задания [Kaggle Competition](https://www.kaggle.com/competitions/uplift-shift-23/overview). 

## Оглавление
[1. Постановка задачи](https://github.com/Vlad15lav/uplift-model#постановка-задачи)  
[2. Описание данных](https://github.com/Vlad15lav/uplift-model#описание-данных)  
[3. Метрика оценивания](https://github.com/Vlad15lav/uplift-model#метрика-оценивания)  
[3. Моделирование](https://github.com/Vlad15lav/uplift-model#моделирование)  
[4. Результат](https://github.com/Vlad15lav/uplift-model#результат)  

## Постановка задачи
Задача кейса является построение модели, предсказывающие поведение целевой аудитории, как изменится поведение людей, если им предложить какое-то воздействие, например, рекламу или скидку. Основная идея заключается в том, чтобы определить, насколько человек изменит свое поведение под воздействием определенной стратегии, в сравнении с тем, если бы этого воздействия не было вообще.

## Описание данных
Для данной задачи использует несколько наборов данных для обучения и тестирования модели Uplift. Все данные представлены в формате CSV. Вот краткое описание каждого файла данных:

1. **clients2.csv:**
   - Содержит информацию о клиентах, включая уникальные идентификаторы и дополнительные характеристики.

2. **products.csv:**
   - Содержит информацию о товарах, включая их уникальные идентификаторы и дополнительные характеристики.

3. **train_purchases.csv:**
   - Предоставляет историю покупок для клиентов, которые включены в обучающий набор данных.

4. **test_purchases.csv:**
   - Предоставляет историю покупок для клиентов, которые включены в тестовый набор данных.

- **train.csv:**
  - `client_id`: Уникальный идентификатор клиента.
  - `treatment_flg`: Была ли совершена маркетинговая коммуникация (1 - да, 0 - нет).
  - `purchased`: Была ли совершена покупка (1 - да, 0 - нет).

- **test.csv:**
  - `client_id`: Уникальный идентификатор клиента.

## Метрика оценивания
Для оценки эффективности моделей Uplift, часто используется метрика, основанная на трансформации классов. Эта метрика позволяет оценить, насколько успешно модель предсказывает эффект воздействия на целевую аудиторию. 
Uplift - это метрика, которая учитывает вероятность совершения покупки в группах treatment (получивших воздействие) и control (контрольной группе).

Целева метрика:  
$$Z_i=Y_i*W_i+(1 - Y_i) * (1 - W_i)$$

$$Z_i = \begin{cases} 
1 & \text{если } W_i = Y_i \\
0 & \text{иначе}
\end{cases}
\$$

- $Z_i$: целевая метрика (class transformation).
- $W_i$: флаг коммуникации `treatment_flg`.
- $Y_i$: реакция клиента `purchased`.

Uplift метрика по формуле:
$$uplift = 2 * P(Z = 1) - 1$$

## Моделирование
Для решения поставленной задачи использовались разнообразные алгоритмы машинного обучения, основанные на деревьях решений, а именно LightGBM, CatBoost, XGBoost и Random Forest.
Для повышения эффективности моделей генерировались признаки на основе истории клиентов. Этот процесс включал в себя анализ предыдущих взаимодействий клиентов, их покупок и реакции на маркетинговые стратегии. 

- **shift-uplift-base.ipynb**: обучение одиночной модели LightGBM.
- **shift-uplift-ensemble.ipynb**: Blending моделей ML.

## Результат
Результаты подходов машинного обучения показана в таблице ниже:

| Model       | Public Test                | Private Test |
| ------------- |:------------------:| -----:|
| LightGBM     | 0.04652    | 0.04919 |
| CatBoost + XGBoost + LightGBM + RF     | 0.05491 |   0.04506 |
