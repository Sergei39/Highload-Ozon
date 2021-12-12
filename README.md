# Ozon

## 1. Тема и целевая аудитория
### 1.1. Тема
[Ozon](https://www.ozon.ru/) - интернет-магазин, сервис для покупки товаров.
### 1.2. Целевая аудитория
За ноябрь 2021 года насчитывает 157 млн посетителей сайта в месяц [[1]](https://www.similarweb.com/website/ozon.ru/) 
или 5 млн посетителей в день.

Можно выделить 3 сегмента стран которые, чаще других посещают Ozon [[2]](https://www.similarweb.com/website/ozon.ru/), 
однако основная масса сосредоточена в России.
* Россия - 93.97%
* Страны Европы - 5%
* США - 0.34%

Возрастная категория: 18-45 лет

### 1.3. MVP
* регистрация и авторизация
* поиск товара по поисковой строке
* фильтрация товаров по категориям
* просмотр списка товаров (39 товаров на одной странице)
* просмотр товара + отзывов (загружается 10 отзывов по умолчанию)
* работа с корзиной (добавление, удаление)
* заказ товара
* написание отзыва

## 2. Расчет нагрузки
### 2.1. Продуктовые метрики

#### 2.1.1. Дневная аудитория
Ежедневно страницу Ozon открывают **5 млн** раз, по итогу ноября 2021 года.
Точных данных о количестве пользователей в день нет, поэтому используя число в 5 млн оценочно расчитаем это число.
Возьмем коэфициент 0.75, получаем `5млн * 0.75 = 3.75млн` уникальных пользователей в день.

#### 2.1.2. Среднее количество действий пользователя
Средняя продолжительность сессии одного пользователя 9 мин. 
За это время пользователь в среднем посещает 11 страниц [[3]](https://www.similarweb.com/website/ozon.ru/)

Используя личный опыт и опыт знакомых, был составлен сценарий стандартного поведения пользователя на сайте.
Стоит отметить что оценко является досточно субъективной,
и количество посещения разных страниц может сильно различаться в зависимости от пользовательского сценария.

* регистрация или авторизация: 0-1 раз (0.5)
* поиск товара по поисковой строке: 2-5 раз (3.5)
* просмотр списка товаров и изменение категорий: 2-5 * 3 раз (10.5)
* просмотр страницы товара: 4-12 раз (8)
* добавление в корзину: 0-4 раз (2)
* удаление из корзины: 0-2 раз (1)
* заказ товара: 0-1 раз (0.5)
* написание отзыва: 0-1 раз (0.5)

### 2.2. Технические метрики
#### 2.2.1. Общий объем изображений товаров
Ozon торгует более 9 млн товарных наименованй [[4]](https://ru.wikipedia.org/wiki/Ozon).
Каждый товар содержит в среднем 4 фотографии, при этом каждое изображение весит в среднем 50 KB.
Отсюда получаем `50 * 4 * 9000000 = 1.67 Тб`.

#### 2.2.2. Общий объем изображений в отзывах товара
* Предположим что из 9 млн, есть 1 млн наиболее активных товаров, у которых среднее количество отзывов 20.
  Учитывая, что к большинству отзывов не прикреплено никакое изображение, 
  а к оставшимся прикреплено гораздо больше картинок, чем одна, 
  возьмем, что в среднем количество изображений прикрепленных к отзыву 1, кторое весит 50 KB.
  Отсюда получаем `1000000 * 20 * 50 = 0.93 Тб`
  
* У оставшихся 8 млн в среднем по 2 отзыва,
  также возьмем в среднем, что к одному отзыву прикреплена 1 изображение.
  Отсюда получаем `8000000 * 1 * 50 = 0.37 Тб`

#### 2.2.2. Сетевой трафик
Основным типом трафика является получение информации о товарах. 
Можно выделить две наиболее посещаемые страницы: детальная страница товара, каталог товаров.

* Детальная страница товара

  Как уже было сказанно выше каждый товар в среднем содержит 4 фотографии по 50 KB, 
  при загрузке страницы также прогружаются отзывы (максимум 10 штук).
  Рассматривая, что в большинстве запросов учавствует 1 млн наиболее активных товаров,
  необходимо еще загрузить 10 изображений по 50 KB.
  То есть в среднем объем трафика составляет 700 KB.
  `700 * 8 * 5000000 / 24 / 3600 = 316 MB/s`.
  
* Каталог товаров

  При просмотре списка товаров, необходимо загрузить 39 изображений товаров,
  однако вес одного изображения составляет 40 KB, так как это лишь превью фото и нет необходимости загружать полностью.
  Получаем 1560 KB которые надо загрузить, учитывая трафик, получаем:
  `1560 * 10.5 * 5000000 / 24 / 3600 = 926 MB/s`
  
#### 2.2.3. Расчет RPS
Пользователь сайта в среднем совершает 27 операций. Получаем: 
`RPS = 27 * 5000000 = 1563`.

Запросы на чтение:
* поиск товара по поисковой строке: **202 RPS**
* просмотр списка товаров и изменение категорий: **608 RPS**
* просмотр страницы товара: **463 RPS**

Запросы на запись:
* регистрация или авторизация: **29 RPS**
* работа с корзиной (добавление, удаление): **174 RPS**
* заказ товара: **29 RPS**
* написание отзыва: **29 RPS**
