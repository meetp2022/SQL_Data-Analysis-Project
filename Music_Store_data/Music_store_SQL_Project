--Q.1) Who is the senior most employee based on the job title?

select * from employee
Order by levels DESC
Limit 1

--Q.2 Which countries have the most invoices?
select count(*) as c, billing_country from invoice
Group by billing_country
Order by c DESC

--Q.3 What are top 3 values of total invoice?
select total from invoice
Order by total DESC
limit 3

--Q4: Which city has the best customers? 
------We would like to throw a promotional Music Festival in the city we made the most money. 
------Write a query that returns one city that has the highest sum of invoice totals. 
------Return both the city name & sum of all invoice totals

select * from invoice
select billing_city, SUM(total) as invoice_total, COUNT(invoice_id) as invoice_count from invoice
Group by billing_city
Order by SUM(total) Desc
Limit 1


--Q5: Who is the best customer? The customer who has spent the most money will be declared the best customer. 
------Write a query that returns the person who has spent the most money.

select * from customer

select c.customer_id, c.first_name, c.last_name, sum(total) as total_money
from 
customer as c INNER JOIN invoice as i
ON c.customer_id = i.customer_id
Group by c.customer_id
Order by total_money Desc
limit 1


--Q.6) Write query to return the email, first name, last name, & Genre of all Rock Music listeners. 
-------Return your list ordered alphabetically by email starting with A

select Distinct email, first_name, last_name
FROM customer
INNER JOIN invoice ON customer.customer_id = invoice.customer_id
INNER JOIN invoice_line ON invoice.invoice_id = invoice_line.invoice_id
WHERE track_id IN (
    Select track_id from track
    JOIN genre ON track.genre_id = genre.genre_id
    WHERE genre.name = 'Rock')
 Order by email; 
 
 
 --Q7: Let's invite the artists who have written the most rock music in our dataset. 
 ------Write a query that returns the Artist name and total track count of the top 10 rock bands

  
 SELECT artist.name, COUNT(track.track_id) as Total_Track
 FROM
 track JOIN album ON track.album_id = album.album_id
 JOIN artist ON artist.artist_id = album.artist_id
 Where track_id IN (
     Select track_id from track JOIN genre 
     ON track.genre_id = genre.genre_id
     Where genre.name like 'Rock')
 GROUP by artist.artist_id  
 Order By Total_Track DESC
 Limit 10
 
 ---Other approach ---- 
 SELECT 
    artist.name, 
    COUNT(track.track_id) AS TotalTrackCount
FROM 
    artist
JOIN album ON artist.artist_id = album.artist_id
JOIN track ON album.album_id = track.album_id
JOIN genre ON track.genre_id = genre.genre_id
WHERE 
    genre.name = 'Rock'
GROUP BY 
    artist.name
ORDER BY 
    TotalTrackCount DESC
LIMIT 10;

 
 
 ---Q8: Return all the track names that have a song length longer than the average song length. 
 -------Return the Name and milliseconds for each track. 
--------Order by the song length with the longest songs listed first.

Select * from track

Select name, milliseconds 
FROM track 
Where milliseconds > (Select AVG(milliseconds) from track)               
ORDER by milliseconds DESC


---Q9: Find how much amount spent by each customer on artists? 
------Write a query to return customer name, artist name and total spent

WITH best_selling_artist AS (
    Select artist.artist_id as artist_id, artist.name as artist_name, SUM(invoice_line.unit_price * invoice_line.quantity) as Total_spent
    From invoice_line
    JOIN track ON invoice_line.track_id = track.track_id
    JOIN album ON album.album_id = track.album_id
    JOIN artist ON artist.artist_id = album.artist_id
    Group by artist.artist_id
    Order by Total_spent DESC
    Limit 1)
    
 SELECT c.customer_id, c.first_name, c.last_name, bsa.artist_name, bsa.Total_spent
 FROM customer c
 JOIN invoice i ON i.customer_id = c.customer_id
JOIN invoice_line il ON il.invoice_id = i.invoice_id
JOIN track t ON t.track_id = il.track_id
JOIN album a on a.album_id = t.album_id

 JOIN best_selling_artist bsa ON bsa.artist_id = a.artist_id
 GROUP by 1,2,3,4,5
 Order by 5 DESC


--Q10: We want to find out the most popular music Genre for each country.
------We determine the most popular genre as the genre with the highest
------amount of purchases. Write a query that returns each country along with
------the top Genre. For countries where the maximum number of purchases
------is shared return all Genres.

Select * from invoice
Select * from invoice_line

With popular_genre as
(
    Select distinct i.billing_country, g.name, Count(il.quantity) as count_purchase,
    ROW_NUMBER() OVER (Partition by i.billing_country ORDER BY Count(il.quantity) DESC) As row_no
    FROM invoice i
    JOIN invoice_line il ON il.invoice_id = i.invoice_id
    JOIN track t ON t.track_id = il.track_id
    JOIN genre g ON g.genre_id = t.genre_id
    Group by 1,2
    Order by 1,3 DESC 
)

Select billing_country, name , count_purchase, row_no from popular_genre where row_no <= 1