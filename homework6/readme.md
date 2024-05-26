# Лаб.работа 6 

## Рассмотрим базу данных для логистической компании

### Преобразование в нормализованную схему:

#### Маршруты (Routes):

id: Идентификатор маршрута. <br>
route_name: Название маршрута.<br>
distance: Дистанция маршрута.<br>
days_on_way: Количество дней в пути.<br>
payment: Оплата за маршрут.<br>

#### Водители (Drivers):

id: Идентификатор водителя.<br>
last_name: Фамилия водителя.<br>
first_name: Имя водителя.<br>
patronymic: Отчество водителя.<br>
driving_experience: Опыт вождения (в годах).<br>

#### Выполненная работа (Work_done):

route_id: Идентификатор маршрута (внешний ключ к таблице Routes).<br>
driver_id: Идентификатор водителя (внешний ключ к таблице Drivers).<br>
departure_date: Дата отправления.<br>
return_date: Дата возвращения.<br>
bonus: Бонус за выполненную работу.


### Определение ключевых атрибутов и минимального покрытия
#### Таблица "Маршруты" (Routes)
Ключевые атрибуты: id <br>
Функциональные зависимости:
id → route_name, distance, days_on_way, payment
#### Таблица "Водители" (Drivers)
Ключевые атрибуты: id <br>
Функциональные зависимости:
id → last_name, first_name, patronymic, driving_experience<br>
#### Таблица "Выполненная работа" (Work_done)
Ключевые атрибуты: (route_id, driver_id, departure_date)<br>
Функциональные зависимости:
(route_id, driver_id, departure_date) → return_date, bonus

### Минимальное покрытие


#### Таблица "Маршруты" (Routes)
Минимальное покрытие: {id → route_name, distance, days_on_way, payment} <br>
#### Таблица "Водители" (Drivers)
Минимальное покрытие: {id → last_name, first_name, patronymic, driving_experience}<br>
#### Таблица "Выполненная работа" (Work_done)
Минимальное покрытие: {(route_id, driver_id, departure_date) → return_date, bonus}<br>


#### "Хорошая" схема базы данных:
#### Маршруты (Routes)

id (ключ)<br>
route_name<br>
distance<br>
days_on_way<br>
payment<br>
#### Водители (Drivers)

id (ключ)<br>
last_name<br>
first_name<br>
patronymic<br>
driving_experience<br>
#### Выполненная работа (Work_done)

route_id (ключевой атрибут)<br>
driver_id (ключевой атрибут)<br>
departure_date (ключевой атрибут)<br>
return_date<br>
bonus<br>

#### В этой схеме:

В таблице "Маршруты" ключевой атрибут - id.<br>
В таблице "Водители" ключевой атрибут - id.<br>
В таблице "Выполненная работа" ключевые атрибуты - (route_id, driver_id, departure_date).<br>
Подведем итог
Мы выделили ключевые атрибуты и нашли минимальное покрытие для каждой таблицы. Затем мы построили "хорошую" схему базы данных, которая обеспечивает целостность данных и нормализована.