# ECOMMMERCE_PROJECT
ECOMMMERCE_PROJECT: FROM DRITY DATA TO CLEAN DATA BY SQL
use bhavana;
use mall;
show tables;
select*from ecommerce_dirty_data;

rename table ecommerce_dirty_data to ECOMMERCE;
SELECT*FROM ECOMMERCE;
-- OR
ALTER TABLE ECOMMERCE RENAME TO ECOMMERCE_DATA;
SELECT*FROM ECOMMERCE_DATA;

-- 1.COLUMN NAME CHANGE 
ALTER TABLE ECOMMERCE_DATA RENAME COLUMN ORDER_ID TO ORDER_SNO;
ALTER TABLE ECOMMERCE_DATA RENAME COLUMN firstname TO FIRST_NAME;
ALTER TABLE ECOMMERCE_DATA RENAME COLUMN lastname TO LAST_NAME;
ALTER TABLE ECOMMERCE_DATA RENAME COLUMN PRODUCT TO PRODUCTS;
ALTER TABLE ECOMMERCE_DATA RENAME COLUMN QUANTITY TO QUANTITY;
ALTER TABLE ECOMMERCE_DATA RENAME COLUMN EMAIL TO EMAIL_ID;
ALTER TABLE ECOMMERCE_DATA RENAME COLUMN PRICE TO PRICES;
ALTER TABLE ECOMMERCE_DATA RENAME COLUMN CITY TO TOWN;
SELECT*FROM ECOMMERCE_DATA;

-- 2.DATA TYPE
DESC ECOMMERCE_DATA; #price,order_date

update ECOMMERCE_DATA set prices=null where prices="";
alter table ECOMMERCE_DATA modify column prices int; 

select*,date_format(str_to_date(order_date,'%d-%m-%Y'),'%Y-%m-%d') from ECOMMERCE_DATA;

begin;
update ECOMMERCE_DATA set order_date=date_format(str_to_date(order_date,'%d-%m-%Y'),'%Y-%m-%d');
commit;
SELECT*FROM ECOMMERCE_DATA;

select distinct products from ECOMMERCE_DATA;
set sql_safe_updates=0;
begin;
update ECOMMERCE_DATA set prices=(
case
when products='camera' then 15000
when products='mouse' then 100
when products='keyboard' then 1000
when products='moblie' then 20000
when products='charger' then 4000
when products='tablet' then 110000
when products='headphones' then 150
when products='laptop' then 75000
end
) where prices is null;
commit;
SELECT*FROM ECOMMERCE_DATA;

select count(email_id) from ECOMMERCE_DATA where email_id is null or email_id="";
select count(town) from ECOMMERCE_DATA where town is null or town ="";
select count(payment_method) from ECOMMERCE_DATA where payment_method is null or payment_method ="";

update  ECOMMERCE_DATA set email_id='NA' where email_id is null or email_id ="";
SELECT*FROM ECOMMERCE_DATA;

select distinct state from ECOMMERCE_DATA;
update ECOMMERCE_DATA set town=(
case
when state='wb' then 'kolkatha'
when state='ka' then 'karnataka'
when state='wb' then 'WestBengal'
when state='ts' then 'HYD'
when state='mh' then 'mumbai'
when state='gj' then 'gujarat'
when state='tn' then 'chennai'
end
) where town is null or town ="";

select distinct town from ECOMMERCE_DATA;
update ECOMMERCE_DATA set town='HYD' where town='Hyderabad';
update ECOMMERCE_DATA set town='Bangalore' where town='Bang';

update ECOMMERCE_DATA set payment_method=null where payment_method="";
update ECOMMERCE_DATA set payment_method='COD' where payment_method IS NULL;

UPDATE ECOMMERCE_DATA SET Payment_method=concat((left(payment_method,1)),lower(substr(payment_method,2)));
