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
<b> Task №17:Определить, сколько потратил в 2005 году каждый из членов семьи. В результирующей выборке не выводите тех членов семьи, которые ничего не потратили.</b>
```
SELECT member_name, status, SUM(Payments.amount * Payments.unit_price) AS costs
FROM FamilyMembers
JOIN Payments ON FamilyMembers.member_id = Payments.family_member
WHERE YEAR(Payments.date) = 2005
GROUP BY member_name, status;
```

<b> Task №18:Выведите имя самого старшего человека. Если таких несколько, то выведите их всех.</b>
```
SELECT member_name
FROM FamilyMembers
WHERE birthday = (
    SELECT MIN(birthday)
    FROM FamilyMembers);
```
<b> Task №19:Определить, кто из членов семьи покупал картошку (potato)</b>
```
select DISTINCT status from FamilyMembers
JOIN Payments
ON FamilyMembers.member_id=Payments.family_member
JOIN Goods
oN Payments.good=Goods.good_id
WHERE Goods.good_name='potato'
```
<b> Task №20:Сколько и кто из семьи потратил на развлечения (entertainment). Вывести статус в семье, имя, сумму.</b>
```
SELECT status, member_name, SUM(Payments.amount * Payments.unit_price) AS costs
FROM FamilyMembers
JOIN Payments ON Payments.family_member = FamilyMembers.member_id
JOIN Goods ON Goods.good_id = Payments.good
JOIN GoodTypes ON GoodTypes.good_type_id = Goods.type
WHERE GoodTypes.good_type_name = 'entertainment'
GROUP BY status, member_name;
```
<b> Task №21:Определить товары, которые покупали более 1 раза.</b>
```
SELECT good_name
FROM Goods
JOIN Payments ON Goods.good_id = Payments.good
GROUP BY good_name
HAVING COUNT(Payments.good) > 1;
```
<b> Task №22:Найти имена всех матерей (mother)</b>
```
SELECT member_name FROM FamilyMembers
WHERE status='mother'
```
<b> Task №23:Найдите самый дорогой деликатес (delicacies) и выведите его цену</b>
```
 SELECT good_name, unit_price 
FROM Payments
JOIN Goods ON Payments.good = Goods.good_id
JOIN GoodTypes ON Goods.type = GoodTypes.good_type_id
WHERE good_type_name = 'delicacies'
LIMIT 1;
```
<b> Task №24:Определить кто и сколько потратил в июне 2005</b>
```
SELECT member_name, SUM(amount * unit_price) as costs FROM FamilyMembers
JOIN Payments
 ON FamilyMembers.member_id=Payments.family_member
WHERE DATE_FORMAT(Payments.date, '%m.%Y') = '06.2005'
GROUP BY FamilyMembers.member_name;
```
<b> Task №25:Определить, какие товары не покупались в 2005 году</b>
```
SELECT good_name
FROM Goods
WHERE good_id NOT IN (
    SELECT good
    FROM Payments
    WHERE YEAR(date) = '2005');
```
<b> Task №26:Определить группы товаров, которые не приобретались в 2005 году</b>
```
SELECT gt.good_type_name
FROM GoodTypes as gt
LEFT JOIN Goods as g ON gt.good_type_id = g.type
LEFT JOIN Payments as p ON g.good_id = p.good AND YEAR(p.date) = 2005
GROUP BY gt.good_type_name
HAVING COUNT(p.payment_id) = 0;
```
<b> Task №27:Определить группы товаров, которые не приобретались в 2005 году</b>
```
SELECT gt.good_type_name
FROM GoodTypes as gt
LEFT JOIN Goods as g ON gt.good_type_id = g.type
LEFT JOIN Payments as p ON g.good_id = p.good AND YEAR(p.date) = 2005
GROUP BY gt.good_type_name
HAVING COUNT(p.payment_id) = 0;
```
<b> Task №28:Узнайте, сколько было потрачено на каждую из групп товаров в 2005 году. Выведите название группы и потраченную на неё сумму. Если потраченная сумма равна нулю, т.е. товары из этой группы не покупались в 2005 году, то не выводите её.</b>
```
SELECT good_type_name, SUM(amount*unit_price) AS costs FROM GoodTypes
JOIN Goods ON good_type_id = type
JOIN Payments ON good = good_id AND YEAR(date) = 2005
GROUP BY good_type_name;
```
<b> Task №28:Сколько рейсов совершили авиакомпании из Ростова (Rostov) в Москву (Moscow) ?</b>
SELECT COUNT(town_from) AS count FROM Trip
WHERE town_from='Rostov' AND town_to='Moscow'

<b> Task №29:Выведите имена пассажиров улетевших в Москву (Moscow) на самолете TU-134. В ответе не должно быть дубликатов.</b>
```
SELECT DISTINCT Passenger.name
FROM Passenger
JOIN Pass_in_trip ON Passenger.id = Pass_in_trip.passenger
JOIN Trip ON Pass_in_trip.trip = Trip.id
WHERE Trip.plane = 'TU-134' 
  AND Trip.town_to = 'Moscow';
```

<b> Task №30:Выведите нагруженность (число пассажиров) каждого рейса (trip). Результат вывести в отсортированном виде по убыванию нагруженности.</b>
```
SELECT trip, COUNT(passenger) AS count
FROM Pass_in_trip
GROUP BY trip
ORDER BY count DESC;
```
<b> Task №31:Вывести всех членов семьи с фамилией Quincey.</b>
```
SELECT * FROM FamilyMembers
WHERE  member_name LIKE '%_Quincey'
```
<b> Task №32:Вывести средний возраст людей (в годах), хранящихся в базе данных. Результат округлите до целого в меньшую сторону.</b>
```
SELECT FLOOR(AVG(TIMESTAMPDIFF(YEAR, birthday, NOW()))) AS age
FROM FamilyMembers;
```
<b> Task №33:Найдите среднюю цену икры на основе данных, хранящихся в таблице Payments. В базе данных хранятся данные о покупках красной (red caviar) и черной икры (black caviar). В ответе должна быть одна строка со средней ценой всей купленной когда-либо икры.</b>
```
SELECT AVG(unit_price) as cost FROM Payments
JOIN Goods ON Payments.good=Goods.good_id
WHERE Goods.good_name='red caviar' OR  Goods.good_name='black caviar'
```
<b> Task №34:Сколько всего 10-ых классов</b>
```
SELECT DISTINCT  COUNT(name) as count FROM Class
WHERE name LIKE '10_%'
```
<b> Task №35:Сколько различных кабинетов школы использовались 2 сентября 2019 года для проведения занятий?</b>
```
SELECT COUNT(DISTINCT classroom) AS count
FROM Schedule
WHERE date LIKE "2019-09-02%"
```
<b> Task №36:Выведите информацию об обучающихся живущих на улице Пушкина (ul. Pushkina)?</b>
```
SELECT * FROM  Student
WHERE address LIKE '%_Pushkina%'
```
<b> Task №37:Сколько лет самому молодому обучающемуся ?</b>
```
SELECT TIMESTAMPDIFF(YEAR,MAX(birthday),NOW()) as year FROM Student
```
<b> Task №38:Сколько Анн (Anna) учится в школе ?</b>
```
SELECT COUNT(first_name) as count FROM Student
WHERE first_name='Anna'
```
<b> Task №39:Сколько обучающихся в 10 B классе ?</b>
```
SELECT COUNT(student) AS count
FROM Student_in_class
JOIN Class ON Student_in_class.class = Class.id
WHERE Class.name = '10 B';
```
<b> Task №40:Выведите название предметов, которые преподает Ромашкин П.П. (Romashkin P.P.). Обратите внимание, что в базе данных есть несколько учителей с такой фамилией.?</b>
```
SELECT name as subjects
FROM Teacher
JOIN Schedule ON Schedule.teacher = Teacher.id
JOIN Subject ON Subject.id = Schedule.subject
WHERE first_name LIKE 'P%'
  AND middle_name LIKE 'P%'
  AND last_name LIKE 'Romashkin';
```
<b> Task №41:Выясните, во сколько по расписанию начинается четвёртое занятие.</b>
```
SELECT start_pair FROM Timepair
WHERE start_pair LIMIT 1 OFFSET 3
```
<b> Task №42:Сколько времени обучающийся будет находиться в школе, учась со 2-го по 4-ый уч. предмет?</b>
```
SELECT DISTINCT TIMEDIFF(
    (
    SELECT end_pair
    FROM Timepair
    WHERE id = '4'),
    (
    SELECT start_pair
    FROM Timepair
    WHERE id = '2')) AS time
FROM Timepair;
```
<b> Task №43:Выведите фамилии преподавателей, которые ведут физическую культуру (Physical Culture). Отсортируйте преподавателей по фамилии в алфавитном порядке.</b>
```
SELECT last_name FROM Teacher
JOIN Schedule ON Teacher.id=Schedule.teacher
JOIN Subject ON Schedule.subject=Subject.id
WHERE Subject.name='Physical Culture'
GROUP BY last_name
ORDER BY last_name ASC 
```
<b> Task №44:Найдите максимальный возраст (количество лет) среди обучающихся 10 классов на сегодняшний день. Для получения текущих даты и времени используйте функцию NOW().</b>
```
SELECT MAX(TIMESTAMPDIFF(YEAR,birthday,CURRENT_DATE)) AS max_year
FROM Student
JOIN Student_in_class ON Student.id = Student_in_class.student
JOIN Class ON Student_in_class.class = Class.id
WHERE Class.name LIKE '10%';
```
<b> Task №45:Какие кабинеты чаще всего использовались для проведения занятий? Выведите те, которые использовались максимальное количество раз.</b>
```
SELECT classroom
FROM Schedule
GROUP BY classroom
HAVING COUNT(classroom) = (
    SELECT COUNT(classroom)
    FROM Schedule
    GROUP BY classroom
    ORDER BY COUNT(classroom) DESC
    LIMIT 1);
```
<b> Task №46:В каких классах введет занятия преподаватель "Krauze" ?</b>
```
SELECT name FROM Class
JOIN Schedule ON Class.id=Schedule.class
JOIN Teacher ON Schedule.teacher=Teacher.id
WHERE last_name='Krauze'
GROUP BY name
```
<b> Task №47:Сколько занятий провел Krauze 30 августа 2019 г.?</b>
```
SELECT COUNT(teacher) AS count
FROM Schedule
WHERE teacher = (
    SELECT id
    FROM Teacher
    WHERE last_name = 'Krauze') AND DATE(date) = '2019-08-30';
```
<b> Task №48:Выведите заполненность классов в порядке убывания</b>
```
SELECT name, COUNT(Student_in_class.student) AS count
FROM Class
JOIN Student_in_class ON Student_in_class.class = Class.id
GROUP BY Class.name
ORDER BY count DESC;
```
<b> Task №49:Какой процент обучающихся учится в "10 A" классе? Выведите ответ в диапазоне от 0 до 100 с округлением до четырёх знаков после запятой, например, 96.0201.</b>
```
SELECT COUNT(student) * 100 / (SELECT COUNT(student) FROM Student_in_class) AS percent
FROM Student_in_class
JOIN Class ON Student_in_class.class = Class.id
WHERE name='10 A';

-- или так 
SELECT ROUND(
(
   SELECT COUNT(*)
   FROM Student_in_class
   WHERE class IN (
       SELECT id
       FROM Class
       WHERE name = '10 A'
   )) * 100.0 / COUNT(*), 4) AS percent
FROM Student_in_class;
```
<b> Task №5:Какой процент обучающихся родился в 2000 году? Результат округлить до целого в меньшую сторону.</b>
```
SELECT 
    FLOOR(COUNT(birthday) * 100.0 / (SELECT COUNT(*) FROM Student)) AS percent
FROM 
    Student
WHERE 
    birthday LIKE '2000%';
```
<b> Task №51:Добавьте товар с именем "Cheese" и типом "food" в список товаров (Goods).</b>
```
INSERT INTO Goods (good_id, good_name, type)
SELECT
    (SELECT COUNT(*) + 1 FROM Goods),
    'Cheese',
    (SELECT good_type_id FROM GoodTypes WHERE good_type_name = 'food');
```
<b> Task №52:Добавьте товар с именем "Cheese" и типом "food" в список товаров (Goods).</b>
```
INSERT INTO GoodTypes (good_type_id, good_type_name)
SELECT
    (SELECT COUNT(*) + 1 FROM GoodTypes),
    'auto';
```
