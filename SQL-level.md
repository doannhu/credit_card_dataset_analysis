INTERMEDIATE

- Joins Inner, Left, Right, Full
- Unions (similar to Full join but 2 tables combine vertically)               
- Case statements
    select name, job_title, salary,
        case when job_title='Engineer' then salary * 1.1
             when job_title='Sales' then salary * 1.05
             else salary * 1.03 
        end as new_salary
    from employee e join employess_salary es on e.employee_id=es.employee_id  
- Updating/Deleting Data
    update employee set age = 30 where name = 'Josh' and job_title = 'Manager'
    delete from employee where employee_id = 96347
- Partition by (Window function)
    select product, date, sum(revenue) over (partition by product order by date)
    from sales;
- Aliasing
- Creating views
- Having & Group By
    Having after Group By and before Order by (no use Where)
    select job_title, avg(salary) from employee e join emp_salary es on e.emp_id=es.emp_id
        group by job_title
        having avg(salary) > 65000
        order by avg(salary)
- Getdate()
    SQL server: select getdate() as current_date_time;
    MySQL: select now() as current_date_time;
    PostgreSQL: select current_timestamp as current_date_time;
- Primary key and Foreign Key


ADVANCED

- CTEs (tables in memory instead of temp table)
    with CTE_employee as (
        select name, gender, avg(salary) over (partition by gender) as avg_salary
            from employee e join emp_salary es 
            on e.employee_id=es.employee_id) 
                select * from CTE_employee
- SYS Tables
- Subqueries
- Temp tables (for faster query if multiple queries using same CTE/subqueries)
    create temporary table temp_table(
        id int, name varchar(50), age int
    )
    insert into temp_table(
        select name, job_title, avg(salary)
            from employee e join emp_salary es on e.emp_id = es.emp_id
    )
- String Functions(TRIM, LTRIM, RTRIM, Replace, Substring)
    trim() to remove white space in string in column:
        select trim(employee_id) as id_trim from employee
    replace() replace a part of a string with new thing. ex: remove '-retired' from last_name:
        select first_name, replace(last_name, '-retired','') as new_last_name
            from employee
    substring() call a substring from column, start at position 1 and go 3 char from position 1:
        select substring(first_name,1,3) from employee

- Regular Expression:
    select * from products where product_name regexp '^A';
- Stored Procedures
- Import Data from different File Types/Sources
- Export Data

COMMON SYMBOLS in Regular Expressions:

    ^: Indicates the start of a string.
    $: Indicates the end of a string.
    .: Matches any single character.
    *: Matches zero or more occurrences of the preceding element.
    +: Matches one or more occurrences of the preceding element.
    ?: Matches zero or one occurrence of the preceding element.
    []: Matches any single character within the brackets.
    |: Acts like a logical OR, allowing for multiple alternatives.


COMMON WINDOW FUNCTIONS:

- ROW_NUMBER(): assign unique number to each row within partition, based on order
    select row_number() over (partition by column_1 order by column_2) as Row,
            column_1, column_2
            from table;

- RANK(): similar to row_number() but give rank to value
    select rank() over(partition by column_1 order by column_2) as rank,
            column_1, column_2 
            from table;

- DENSE_RANK(): similar to rank() but without gaps in ranking for tied values
    select dense_rank() over(partition by column_1 order by column_2) as ds_rank,
            column_1, column_2 
            from table;

- LEAD(): find a column of a row that after the current row
    select employee_id, name, salary,
            lead(salary) over (order by employee_id) as salary_next_employee_ordered_by_id
        from employee_table;

- LAG(): find a column of a row that before the current row
    select employee_id, name, salary,
            lag(salary) over (order by employee_id) as salary_previous_employee_ordered_by_id
        from employee_table;

- SUM(), AVG(), MEAN(), MIN(), MAX()