# Music_store_analysis

---*Easy Question*---
---Q1 Who is senior most employee based on job title

select * from employee
order by levels desc
limit 1



--- Q2 Which country has most invoicess

select count(*) as c , billing_country
from invoice
group by billing_country
order by c desc

select count( billing_country) as c , billing_country
from invoice
group by billing_country
order by c desc

---Q3 what are the top 3 values of total invoices

select total 
from invoice
order by total desc
limit 3

--- Q4 Which city has the best customers? We would like to throw a promotional Music 
--- Festival in the city we made the most money. Write a query that returns one city that 
--- has the highest sum of invoice totals. Return both the city name & sum of all invoice 
--- totals

select
sum(total) as invoice_total,
billing_city
from invoice
group by billing_city
order by invoice_total desc
limit 1

--- Q5 Who is the best customer? The customer who has spent the most money will be 
---    declared the best customer. Write a query that returns the person who has spent the 
---    most money

select sum(i.total) as total,
	   c.customer_id,
	   c.first_name,
	   c.last_name
from customer as c
join invoice as i
on c.customer_id = i.customer_id
group by c.customer_id,c.first_name,c.last_name
order by total desc
limit 1

---*Moderate Question*---

	
---Q1 Write query to return the email, first name, last name, & Genre of all Rock Music 
---listeners. Return your list ordered alphabetically by email starting with A 


select distinct email,first_name,last_name
from customer
join invoice 
on customer.customer_id = invoice.customer_id
join invoice_line 
on invoice_line.invoice_id = invoice.invoice_id
where track_id IN(
	select track_id from track
	join genre 
	on track.genre_id = genre.genre_id
	where genre.name like 'Rock'
)
order by email

---Q2Let's invite the artists who have written the most rock music in our dataset. Write a 
---query that returns the Artist name and total track count of the top 10 rock bands

select artist.artist_id,artist.name,count(artist.artist_id) as number_of_songs 
from track
join album 
on album.album_id = track.album_id
join artist
on artist.artist_id = album.artist_id
join genre
on genre.genre_id = track.genre_id
where genre.name like 'Rock'
group by artist.artist_id
order by number_of_songs desc
limit 10


---Q3Return all the track names that have a song length longer than the average song length. 
---Return the Name and Milliseconds for each track. Order by the song length with the 
---longest songs listed first 
---select * from track

select name,milliseconds 
from track
where milliseconds >(
	select avg(milliseconds) as Avg_track_length
	from track) 
order by milliseconds desc


