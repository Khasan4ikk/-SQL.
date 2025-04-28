# -SQL.
Решение задач по SQL. Список задач https://sql-academy.org/ru/trainer

<b>Task №1: Вывести имена всех когда-либо обслуживаемых пассажиров авиакомпаний.</b>
```
SELECT name
FROM passenger;
```
<b>Task №2: Вывести названия всеx авиакомпаний.</b>
```
SELECT name
FROM Company;
```
<b>Task №3: Вывести все рейсы, совершенные из Москвы.</b>

```
SELECT *
FROM Trip
WHERE town_from = 'Moscow';
```
<b>Task №4: Вывести имена людей, которые заканчиваются на "man".</b>
```
SELECT name
FROM passenger
WHERE name LIKE '%man';
```
<b>Task №5: Вывести количество рейсов, совершенных на TU-134.</b>
```
SELECT COUNT(*) as count
FROM trip
WHERE plane = 'TU-134';
```
<b>Task №6: Какие компании совершали перелеты на Boeing.</b>
```
SELECT DISTINCT name
FROM Company, Trip
WHERE plane = 'Boeing' AND Company.id = Trip.company;
```
<b>Task №7: Вывести все названия самолётов, на которых можно улететь в Москву (Moscow).</b>
```
SELECT DISTINCT name
FROM Company, Trip
WHERE plane = 'Boeing' AND Company.id = Trip.company;
```
<b>Task №8: В какие города можно улететь из Парижа (Paris) и сколько времени это займёт?</b>
```
SELECT town_to, TIMEDIFF(time_in, time_out) AS flight_time
FROM trip
WHERE town_from = 'Paris';
```
<b>Task №9: Какие компании организуют перелеты из Владивостока (Vladivostok)?</b>
```
SELECT DISTINCT name
FROM company, trip
WHERE trip.company = company.id AND town_from = 'Vladivostok';
```
<b>Task №10: Вывести вылеты, совершенные с 10 ч. по 14 ч. 1 января 1900 г.</b>
```
SELECT *
FROM trip
WHERE time_out BETWEEN '1900-01-01T10:0:00.000Z' AND '1900-01-01T14:0:00.000Z';
```
<b>Task №11: Выведите пассажиров с самым длинным ФИО. Пробелы, дефисы и точки считаются частью имени.</b>
```
SELECT name FROM Passenger
ORDER BY LENGTH(name) DESC LIMIT 1;

-- или так
SELECT name
FROM passenger
WHERE LENGTH(name) = (
    SELECT MAX(LENGTH(name))
    FROM passenger);
```
<b>Task №12: Выведите идентификаторы всех рейсов и количество пассажиров на них. Обратите внимание, что на каких-то рейсах пассажиров может не быть. В этом случае выведите число "0".</b>
```
SELECT  
   t.id , 
    COUNT(pt.passenger) AS count 
FROM  
    Trip as t 
LEFT JOIN  
    Pass_in_trip as pt ON t.id   = pt.trip 
GROUP BY  
   t.id ;
```
<b> Task №13:Вывести имена людей, у которых есть полный тёзка среди пассажиров</b>
```
SELECT name
FROM passenger
GROUP BY name
HAVING COUNT(*) > 1;
```
<b> Task №14:В какие города летал Bruce Willis</b>
```
SELECT town_to
FROM Trip
JOIN Pass_in_trip ON Pass_in_trip.trip = Trip.id
JOIN Passenger ON Pass_in_trip.passenger = Passenger.id
WHERE Passenger.name = 'Bruce Willis';
```

<b> Task №15:Выведите идентификатор пассажира Стив Мартин (Steve Martin) и дату и время его прилёта в Лондон (London)</b>
```
SELECT t.time_in, p.id
FROM Trip as t, Passenger  as p, Pass_in_trip  as pt
WHERE t.id = pt.trip
AND pt.passenger = p.id
AND p.name = 'Steve Martin'
AND t.town_to = 'London';
```
<b> Task №16:Вывести отсортированный по количеству перелетов (по убыванию) и имени (по возрастанию) список пассажиров, совершивших хотя бы 1 полет.</b>
```
SELECT passenger.name, COUNT(Pass_in_trip.passenger) AS count
FROM Pass_in_trip
JOIN Passenger ON Pass_in_trip.passenger = Passenger.id
GROUP BY passenger.name
HAVING count >= 1
ORDER BY count DESC, passenger.name ASC;
```
