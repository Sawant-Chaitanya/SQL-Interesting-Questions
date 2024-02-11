```SQL
--sample table to solve the question
create table billing
(
      customer_id               int
    , customer_name             varchar(1)
    , billing_id                varchar(5)
    , billing_creation_date     date
    , billed_amount             int
);

insert into billing values (1, 'A', 'id1', to_date('10-10-2020','dd-mm-yyyy'), 100);
insert into billing values (1, 'A', 'id2', to_date('11-11-2020','dd-mm-yyyy'), 150);
insert into billing values (1, 'A', 'id3', to_date('12-11-2021','dd-mm-yyyy'), 100);
insert into billing values (2, 'B', 'id4', to_date('10-11-2019','dd-mm-yyyy'), 150);
insert into billing values (2, 'B', 'id5', to_date('11-11-2020','dd-mm-yyyy'), 200);
insert into billing values (2, 'B', 'id6', to_date('12-11-2021','dd-mm-yyyy'), 250);
insert into billing values (3, 'C', 'id7', to_date('01-01-2018','dd-mm-yyyy'), 100);
insert into billing values (3, 'C', 'id8', to_date('05-01-2019','dd-mm-yyyy'), 250);
insert into billing values (3, 'C', 'id9', to_date('06-01-2021','dd-mm-yyyy'), 300);
```
