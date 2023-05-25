# MyFirstDataProject
Проект для трека https://ods.ai/tracks/my_first_data_project

В данном проекте реализуется телеграм-бот, который рекомендует настольные игры и по желанию позволяет купить понравившуюся игру. Этот бот может помочь вам выбрать настольную игру, предлагая рекомендации на основе популярности, сходства с другой игрой или персональных предпочтений, если у вас есть учетная запись (добавление новых учетных записей пока не предусмотрено). Вы также можете запросить случайно выбранную игру. Важно отметить, что названия и описания игр только на английском языке.

Сам телеграм-бот - https://t.me/Board_Recommender_Bot 

Для обучения модели использовалась библиотека Implicit для построения рекомендательных систем на основе датасетов с неявным таргетом.

<b>Таблица сравнения используемых моделей:</b>
<img width="1004" alt="Таблица сравнения" src="https://github.com/e-dracheva/MyFirstDataProject/assets/122459598/7b75945b-8796-40ef-997e-d9aba6f3c3c2">

<img width="941" alt="Таблица LightFM" src="https://github.com/e-dracheva/MyFirstDataProject/assets/122459598/d76f7d37-a7c6-42ed-9945-dcf0be968357">


nearest_neighbours (CosineRecommender, BM25Recommender, TFIDFRecommender) - метод ближайшего соседа – поиск похожих объектов для всех объектов, с которыми пользователь уже взаимодействовал и выдача топа из этого списка.

ALS предсказывает не исходное значение взаимодействия, а предсказывает факт такого взимодействия. Для каждой пары пользователь-объект есть вес, даже для неизвестных пар.

По функционалу качества чуть лучше показала себя AlternatingLeastSquares(factors = 16, iterations = 30), ее и будем использовать для MVP.


<h3>Структура проекта:</h3>

<pre><code> 
├── database                                  # папка с базами данных <br>
│   ├── user_data.sql                         # база данных с данными пользователей для входа в УЗ
├── datasets                                  # папка с датасетами
│   ├── bgg_boardgames_top_2000.feather       # набор с данными о настольных играх
│   ├── bgg_ratings_top_2000.feather          # набор с данными о пользователях и их взаимодействиями с играми
├── models                                    # папка с моделями
│   ├── als_models.npz                        # модель на основе ALS, которую мы сохранили для использования в боте
├── notebooks                                 # папка c ноутбуками
│   ├── board_game_recommender_notebook.ipynb # ноутбук на котором обучали и тестировали модели, смотрели метрики
├── Презентация                               # папка с презентацией проекта (формат pdf)
├── .gitignore                                # содержит файлы, которые не должны попасть в Git
├── Dockerfile                                # файл для поднятия сервиса в Docker 
├── README.md                                 # описание проекта
├── bot.py                                    # файл разработки бота
├── requirements.txt                          # файл с зависимостями
</code></pre>

<h3>Как запустить сервис через Docker:</h3>

1. Образ есть на <a href="https://hub.docker.com/r/eidracheva/my_bot">Docker Hub<a>, поэтому можно скопировать его себе
<pre><code>docker pull eidracheva/my_bot</code></pre>
2. Обратите внимание, что внутри есть переменные окружения, поэтому для запуска нужно указать токен телеграм-бота в переменную TOKEN
<pre><code>docker run --rm -e TOKEN=*YOUR_TOKEN HERE* eidracheva/my_bot</code></pre>

У бота пока нет возможности регистрировать новых пользователей, поэтому для проверки функционала можно использовать тестового пользователя:
  happyjosiah, пароль: 1234

