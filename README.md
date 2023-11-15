## Рекомендательная система - Документация

### Общее описание:

Проект представляет собой рекомендательную систему, предназначенную для рекомендации товара пользователям. В системе использовались три модели: LightFM, ALS (Alternating Least Squares), и SVD (Singular Value Decomposition). Разработка велась на двух наборах данных: "events" и "combined_data". В результате проведенных экспериментов были получены метрики Precision@3 для каждой модели и набора данных. На основании полученного результата выбрана модель с лучшей метрикой.

### Входные данные для обучения
Данные для обучения представлены в формате CSV и включают следующие поля:

    visitorid: Идентификатор пользователя;
    itemid: Идентификатор элемента (контента);
    event_encoded: Закодированное событие (целевая переменная).

### Трансформации исходного датасета

Для обучения моделей проведены следующие трансформации:

    1. Преобразование timestamp в datetime;
    2. Создание ряда временных признаков (минуты, часы и т.д.);
    3. Удаление лишних и/или неинформативных признаков;
    4. Отсеивание пользователей с малым количеством действий;
    5. Обработка пустых значений;
    6. Кодирование признаков;
    7. Разделение данных;
    8. Извлечение пользовательских идентификаторов, идентификаторов элементов и закодированных событий.

### Построение валидации

Для оценки качества моделей данные были разделены на обучающий и тестовый наборы в соотношении 70:30. Валидация проводилась на тестовом наборе для каждой модели.

### Эксперименты
Для оценки моделей использовалась метрика Precision@3.
#### LightFM

    Тип модели: Hybrid (Collaborative Filtering + Content-Based)
    Гиперпараметры: no_components=100, learning_rate=0.1, loss='warp'
    Precision@3 (events): 5.39%
    Precision@3 (combined data): 6.48%

#### ALS

    Тип модели: Implicit Alternating Least Squares
    Гиперпараметры: factors=100, iterations=100, regularization=0.1
    Precision@3 (events): 0.61%
    Precision@3 (combined data): 1.34%

#### SVD

    Тип модели: Singular Value Decomposition
    Гиперпараметры: По умолчанию
    Precision@3 (events): 19.56%
    Precision@3 (combined data): 10.64%

### Команды для запуска Docker-образа

```bash
docker run -p 5000:5000 aeabramov/recommendations-app:latest
```

### API сервиса
Endpoint: /recommend

    Метод: POST
    Параметры запроса:
        visitor_id: Идентификатор пользователя (integer)

Пример запроса:

```json
{
  "visitor_id": 123
}
```
```powershell
Invoke-RestMethod -Uri http://127.0.0.1:5000/recommend -Method Post -Headers @{"Content-Type"="application/json"} -Body '{"visitor_id": 123}'
```

Пример ответа:

```json
{
  "visitor_id": 123,
  "recommendations": [
    {"item_id": 456, "estimation": 2.13},
    {"item_id": 789, "estimation": 1.95},
    {"item_id": 101, "estimation": 1.87}
  ]
}
```

### Дополнительные команды
Снятие метрик

```bash
curl -X GET http://localhost:5000/metrics
```
