--# MYSQL
--Using the sakila database

USE sakila;

SELECT first_name, last_name FROM actor;

--1a.
ALTER TABLE actor
ADD column actor_name VARCHAR(50);

--1b. 
SET SQL_SAFE_UPDATES=0;
UPDATE actor SET actor_name = UPPER(CONCAT(first_name,' ',last_name));
SELECT actor_name FROM actor;
SET SQL_SAFE_UPDATES=1;

--2a.
SELECT actor_id, first_name, last_name FROM actor
WHERE first_name = 'Joe';

--2b.
SELECT * FROM actor 
WHERE last_name LIKE '%GEN%';

--2c.
SELECT * FROM actor
WHERE last_name LIKE '%LI%'
ORDER BY last_name, first_name;

--2d.
SELECT country_id, country FROM country
WHERE country IN ('Afghanistan', 'Bangladesh', 'China');

--3a.
ALTER table actor
ADD column middle_name VARCHAR(30)
AFTER first_name; 

--3b.
ALTER table actor
MODIFY middle_name BLOB;

--3c.
ALTER table actor
DROP column middle_name;

--4a.

SELECT count(*), last_name FROM actor
GROUP BY last_name;

--4b.
SELECT count(*), last_name FROM actor
GROUP BY last_name
HAVING count(*)>1;

--4c.
SELECT * FROM actor
WHERE actor_name = 'GROUCHO WILLIAMS';

UPDATE actor
SET actor_name = 'HARPO WILLIAMS'
WHERE actor_id = 172;

--4d.
UPDATE actor
SET first_name = 'HARPO'
WHERE actor_id = 172;

UPDATE actor
SET actor_name = 'GROUCHO WILLIAMS'
WHERE actor_id = 172;

UPDATE actor
SET first_name = 'GROUCHO'
WHERE actor_id = 172;

--5a.
CREATE schema IF NOT EXISTS address;

--6a.
SELECT first_name, last_name, address
FROM staff
LEFT JOIN address ON staff.address_id = address.address_id;

--6b.
SELECT first_name, last_name, SUM(amount) AS total_amount, payment_date
FROM staff
LEFT JOIN payment ON staff.staff_id = payment.staff_id
WHERE payment_date LIKE '2005-08%'
GROUP BY staff.staff_id;

SELECT DISTINCT staff_id FROM payment
WHERE payment_date LIKE '2005-08%';

--6c.
SELECT COUNT(actor_id), title
FROM film
INNER JOIN film_actor ON film.film_id = film_actor.film_id
GROUP BY film.title;

--6d.
SELECT COUNT(inventory_id), title
FROM inventory
LEFT JOIN film ON inventory.film_id = film.film_id
WHERE title = 'Hunchback Impossible';

--6e.
SELECT SUM(amount) AS total_paid, last_name
FROM customer
LEFT JOIN payment ON customer.customer_id = payment.customer_id
GROUP BY customer.last_name;

--7a.
SELECT title, `name`
FROM film
LEFT JOIN `language` ON film.language_id = `language`.language_id
WHERE title LIKE 'M%' OR title LIKE 'K%';

--7b.
SELECT actor_name, title
FROM actor
LEFT JOIN film_actor ON actor.actor_id = film_actor.actor_id
LEFT JOIN film on film_actor.film_id = film.film_id
WHERE title = 'Alone Trip';

--7c.
SELECT country, first_name, last_name, email
FROM customer
LEFT JOIN address on customer.address_id = address.address_id
LEFT JOIN city ON address.city_id = city.city_id
LEFT JOIN country ON city.country_id = country.country_id
WHERE country = 'Canada'; 

--7d.

SELECT * FROM category;

SELECT title FROM film
LEFT JOIN film_category ON film.film_id = film_category.film_id
WHERE film_category.category_id = 8;

--7e.
SELECT title FROM film
LEFT JOIN inventory ON film.film_id = inventory.inventory_id
LEFT JOIN rental ON inventory.inventory_id = rental.inventory_id
ORDER BY rental.rental_date;

--7f.
SELECT amount FROM payment
LEFT JOIN store ON payment.last_update = store.last_update
GROUP BY store.store_id;

SELECT store.store_id, SUM(amount)
FROM store
INNER JOIN staff
ON store.store_id = staff.store_id
INNER JOIN payment p 
ON p.staff_id = staff.staff_id
GROUP BY store.store_id
ORDER BY SUM(amount);

-- 7g. 

USE Sakila;

SELECT s.store_id, city, country
FROM store s
INNER JOIN customer cu
ON s.store_id = cu.store_id
INNER JOIN staff st
ON s.store_id = st.store_id
INNER JOIN address a
ON cu.address_id = a.address_id
INNER JOIN city ci
ON a.city_id = ci.city_id
INNER JOIN country coun
ON ci.country_id = coun.country_id;
WHERE country = 'CANADA' AND country = 'AUSTRAILA';

-- 7h.
USE Sakila;

SELECT name, SUM(p.amount)
FROM category c
INNER JOIN film_category fc
INNER JOIN inventory i
ON i.film_id = fc.film_id
INNER JOIN rental r
ON r.inventory_id = i.inventory_id
INNER JOIN payment p
GROUP BY name
LIMIT 5;

-- 8a. 

USE Sakila;

CREATE VIEW top_five_grossing_genres AS

SELECT name, SUM(p.amount)
FROM category c
INNER JOIN film_category fc
INNER JOIN inventory i
ON i.film_id = fc.film_id
INNER JOIN rental r
ON r.inventory_id = i.inventory_id
INNER JOIN payment p
GROUP BY name
LIMIT 5;

-- 8b. 

SELECT * FROM top_five_grossing_genres;

-- 8c. 

DROP VIEW top_five_grossing_genres;
