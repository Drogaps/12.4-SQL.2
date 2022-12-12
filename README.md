# Домашнее задание к занятию "`12.4 Реляционные базы данных: SQL. Часть 2`" - `Кузнецов Андрей`
### Задание 1
Одним запросом получите информацию о магазине, в котором обслуживается более 300 покупателей и выведите в результат следующую информацию:

*фамилия и имя сотрудника из этого магазина,
*город нахождения магазина,
*количество пользователей, закрепленных в этом магазине.

```
mysql> SELECT  concat(st.last_name, ' ', st.first_name) AS "Seller's name", ci.city,  s.store_id, count(c.customer_id) AS "Total buyers"
    -> FROM store s 
    -> JOIN customer c ON c.store_id = s.store_id
    -> JOIN staff st ON st.store_id = s.store_id
    -> JOIN address a ON a.address_id = s.address_id
    -> JOIN city ci ON ci.city_id = a.city_id
    -> GROUP BY s.store_id, ci.city_id, st.staff_id 
    -> HAVING count(c.customer_id) > 300;
+---------------+------------+----------+--------------+
| Seller's name | city       | store_id | Total buyers |
+---------------+------------+----------+--------------+
| Hillyer Mike  | Lethbridge |        1 |          326 |
+---------------+------------+----------+--------------+
1 row in set (0.06 sec)
```
---

### Задание 2
Получите количество фильмов, продолжительность которых больше средней продолжительности всех фильмов.

```
mysql> SELECT count(film_id) AS 'Total films' FROM (SELECT *, AVG(length) OVER() AS avg_films FROM film) agf  WHERE  avg_films < length;
+-------------+
| Total films |
+-------------+
|         489 |
+-------------+
1 row in set (0.01 sec)
```
---

### Задание 3
Получите информацию, за какой месяц была получена наибольшая сумма платежей и добавьте информацию по количеству аренд за этот месяц.

```
SELECT MONTH(p.payment_date) AS Month, SUM(p.amount) AS 'Total sum', count(p.rental_id)  AS 'Number of rentals' from payment p GROUP BY MONTH(p.payment_date) ORDER BY sum(p.amount) DESC LIMIT 1;
+-------+-----------+-------------------+
| Month | Total sum | Number of rentals |
+-------+-----------+-------------------+
|     7 |  28368.91 |              6709 |
+-------+-----------+-------------------+
1 row in set (0.01 sec)
```
---
