# Highload-Ozon
### РПЗ по предмету "проектирование высоконагруженных систем"

## Тема и целевая аудитория
### Тема курсовой
Озон - интернет магазин, сервис для покупки товаров.
### Целевая аудитория
В мае 2021 года число активных покупателей Озон составило 16 миллионов человек. Покупатели находятся в России.
### MVP
* Просмотр товаров.  
* Покупка товаров.
* Оставление отзывов о продавце
## Расчет нагрузки

### Продуктовые метрики
Ежемесячно Озон посещает более **60 миллионов** клиентов, по итогу ноября 2020 года. В качестве дневной аудитории возьмем **4
миллиона** человек, данные сформировались опираясь на дневную аудиторию Wilberries, так как для Озона эти числа не
опубликованы.
* В среднем в описание каждого товара содержится
  * 4 фотографий от продовца. В среднем каждая фотография размером 300 Кб
  * 25 фотографий от покупателей. В среднем каждая фотография размером 300 Кб  
  * 15 текстовых характеристик (название + значение = 150 Байт)
  * название и описание товара (5000 Байт)  
    Исходя из этого средний размер хранилища для одного товара примерно равен 9 Мб
* В среднем, при одном заходе каждый пользователь просматривает 18 различных страниц, возьмем это значение как количество страниц с товаром
* Среднее значение заказов в сутки составляет 400 тысяч. Исходя из расчетов, 0.1 заказов на одного пользователя в день.
* Предположим, что каждый 5 покупатель оставил отзыв о товаре. 0.05 комментариев на одного пользователя в день
  * В правилах запрещено прекреплять больше 10 фотографий за один отзыв. Возьмем среднее значение в 5 фотографий за отзыв
* К маю 2021 года на Озоне размещено 19 миллионов различных товаров

### Технические метрики
* Если рассматривать что на Озоне на текущий момент размещено 19 миллионов товаров, каждое из которых занимает 7 Мб, получаем 163 Тб
* Сетевой трафик (есть 2 варианта):
  * Загрузка товара пользователю. Получаем 7 Гб/c
  * Отзыв о товаре. 4 Мб/c
* RPS:
  * Просмотр товара: 72 млн. в день => 833 RPS
  * Покупка товара: 400 тыс. в день => 6 RPS
  * Отзыв: 3 RPS