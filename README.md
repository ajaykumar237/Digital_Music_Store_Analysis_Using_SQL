


# Digital Music Store Analysis Using SQL

### Project Overview
This data analysis project aims to provide the solutions to the business problems/ questions related to the store performance. 

### Data Sources
Music Data: The primary dataset used for this analysis is the "digitalmusic_store.sql" file, containing detailed information about song, genre, singer, country, album etc. available on the digital store.

### Tools
- SQL - Data Analysis
  
### Data Analysis



1) Who is the senior most employee based on job title?

```
SELECT first_name, last_name, title, levels
FROM employee
ORDER BY 4 DESC
LIMIT 1;
```

2) Which country have the most invoices?
```
SELECT billing_country, COUNT(*)
FROM invoice
GROUP BY 1
ORDER BY 2 DESC;
```

3) What are the top 3 values of total invoice?
```
SELECT total
FROM invoice
ORDER BY 1 DESC
LIMIT 3;
```
4) Which city has the best customers? We would like to through a promotional music festival in the city we made the most money. Write a query that returns one city that has the highest sum of invoice totals. Return both the city name and sum of all invoice totals.
```
SELECT billing_city, SUM(total) as invoice_total
FROM invoice
GROUP BY 1
ORDER BY 2 DESC
LIMIT 1;
```

5) Who is the best customer? The customer who has spent the most money will be declared the best customer. Write a query that returns the person who has spent the most money.
```
SELECT c.customer_id, first_name, last_name, SUM(i.total)
FROM customer c
JOIN invoice i
ON c.customer_id = i.customer_id
GROUP BY 1
ORDER BY 4 DESC
LIMIT 1;
```

6) Write a query to return the email, first name, last name & genre of all rock music listeners. Return your list order alphabetically by email starting with A.
```
SELECT DISTINCT email, first_name, last_name
FROM customer c
JOIN invoice i ON c.customer_id = i.customer_id
JOIN incoice_line il ON i.invoice_id = il.invoice_id
WHERE track_id IN (
                  SELECT track.id FROM track t
		  JOIN genre g ON t.genre_id = g.genre_id
		  WHERE g.name LIKE 'Rock'
		  )
ORDER BY 1;
```
7) Let's invite the artists who have written the most rock music in our dataset. Write a query that returns the artist name and total track count of the top 10 rock bands.
```
SELECT a.artist_id, a.name, COUNT(a.artist_id) as number_of_songs
FROM artist a
JOIN album al ON a.artist_id = al.artist_id
JOIN track t ON al.album_id = t.album_id
JOIN genre g ON t.genre_id = g.genre_id
WHERE g.name LIKE 'Rock'
GROUP BY 1
ORDER BY 3 DESC
LIMIT 10;
```
8) Return all the track names that have a song length longer than the average song length. Return the name and milliseconds for each track. Order by the song length with the longest songs listed first.
```
SELECT name, milliseconds
FROM track
WHERE milliseconds > (
                      SELECT AVG(milliseconds)
                      FROM track
                      )
ORDER BY milliseconds DESC;
```
9) Find how much amount spend by each customer on artists? Write a query to return customer name, artist name and total spent.
```
WITH top_selling_artist as (
                           SELECT a.artist_id, a.name, SUM(il.unit_price*quantity) as total_sales
                           FROM invoice_line il
                           JOIN track t ON il.track_id = t.track_id
                           JOIN album al ON t.album_id = al.album_id
		           JOIN artist a ON al.artist_id = a.artist_id
                           GROUP BY 1
                           ORDER BY 3 DESC
                           LIMIT 1
                           )
SELECT CONCAT(c.first_name, last_name) customer_name, tsa.artist_name, SUM(il.unit*quantity) total spent
FROM customer c
JOIN invoice i ON c.customer_id = i.customer_id
JOIN invoice_line il ON i.invoice_id = il.invoice_id
JOIN track t ON il.track_id = t.track_id
JOIN album al ON t.album_id = al.album_id
JOIN top_selling_artist tsa ON al.artist_id = tsa.artist_id
GROUP BY 1,2
ORDER BY 3 DESC;
```




    
