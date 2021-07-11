# Assessment-Answers

This documentation is to answer question no 1. 
1.	Show list of transactions occurring in February 2018 with SHIPPED status.

select *
from finaccel.transaction
where status = 'SHIPPED' and transaction_date >= '01-02-2018' and transaction_date <= '28-02-2018';

2.	Show list of transactions occurring from midnight to 9 AM

select *
from finaccel.transaction
where status = 'SHIPPED' and 
EXTRACT(HOUR FROM transaction_date) >= '00:00:00' and EXTRACT(HOUR FROM transaction_date) <= '09:00:00' ;

3.	Show a list of only the last transactions from each vendor
	
select *
from finaccel.transaction a
where transaction_date = 
(
 	  Select max( transaction_date )
 	  from finaccel.transaction  b
 	  where a.vendor = b.vendor )
order by vendor ;

4.	Show a list of only the second last transactions from each vendor

select *
from (
select *,
             row_number() over (partition by vendor order by transaction_date desc) as rownumber
      from finaccel.transaction t
     ) t
    where rownumber = 2;

5.	Count the transactions from each vendor with the status CANCELLED per day

select cast(transaction_date AS date) as date, count(*) as counter
from finaccel.transaction
where status = 'CANCELLED'
group by date;
6.	Show a list of customers who made more than 1 SHIPPED purchases

select customer_id , 
count(order_id) AS counter
from finaccel.transaction
where status = 'SHIPPED'
group by customer_id
having counter>1;

7.	Show the total transactions (volume) and category of each vendors by following these criteria:
A.	Superb: More than 2 SHIPPED and 0 CANCELLED transactions
B.	Good: More than 2 SHIPPED and 1 or more CANCELLED transactions
C.	Normal: other than Superb and Good criteria
Order the vendors by the best category (Superb, Good, Normal)

Select vendor, count(*) AS total_orders,
Case 
	When sum(status='SHIPPED') > 2 then 'Superb'
When sum(status ='SHIPPED')> 2 AND sum(status = 'CANCELLED') >=1 then 'Good'
Else 'Normal'
	End 
 		   AS classification
from finaccel.transaction
Group by vendor
order by total_orders desc;

8. Group the transactions by hour of transaction_date

select extract(HOUR from transaction_date) as hour_of_the_day, count(*) as total_transactions
from finaccel.transaction
Group by extract(HOUR from transaction_date)
;

9. Group the transactions by day and statuses as the example below

select extract(day from transaction_date) as date, 
sum(status='SHIPPED') as SHIPPED, 
sum(status='CANCELLED') as CANCELLED, 
sum(status='PROCESSING') as PROCESSING
from finaccel.transaction
group by date;
10. Calculate the average, minimum and maximum of days interval of each transaction (how many days from one transaction to the next)

Part 2
1.	Show the sum of the total value of the products shipped along with the Distributor Commissions (2% of the total product value if total quantity is 100 or less, 4% of the total product value if total quantity sold is more than 100)

select product_name, 
quantity*price as value,
case 
when quantity < 100 then format(.2*quantity*price, 0)
when quantity > 100 then format(.4*quantity*price, 0)
end as distribution_commission
from finaccel.transaction_details;

2.	Show total quantity of “Indomie (all variant)” shipped within February 2018

Select sum(td.quantity)
From finaccel.transaction as t
Join
finaccel.transaction_details as td
on tt.id = td.id
where td.product_name like ‘indomie%’ 
and tt.status = ‘SHIPPED’
and tt.transaction_date <= ‘18-02-01’ 
and tt.transaction_date >= ‘18-29-01’

3.	For each product, show the ID of the last transaction which contained that particular product

Select product, id ‘Last Transaction ID’
From 
(
Select product, id ,
row_number() over (
partition by product
Order by transaction_date DESC) row_num 
where row_num = 1
group by product;
