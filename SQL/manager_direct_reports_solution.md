# Manager and Direct Reports --- PySpark/SQL Problem

## Problem

Given an employee hierarchy table, write a PySpark query to list each
manager along with the count of employees reporting to them (direct
reports only).

------------------------------------------------------------------------

##  Employee Table (Sample Data)

  emp_id   emp_name   manager_id
  -------- ---------- ------------
  1        John       NULL
  2        Alice      1
  3        Bob        1
  4        David      2
  5        Emma       2
  6        Chris      3

------------------------------------------------------------------------

## My Solution (SQL)

``` sql
with ct as(
select 
manager_id, count(*) as Number_of_employee
from employee_hierarchy
group by manager_id
),
ct1 as(
select emp_id, emp_name, manager_id from employee_hierarchy
)
select ct.manager_id,
ct1.emp_name as manager_name,
ct.Number_of_employee
from ct1
join ct on ct.manager_id=ct1.emp_id 
;
```

------------------------------------------------------------------------

## Output

  manager_id   manager_name   Number_of_employee
  ------------ -------------- --------------------
  1            John           2
  2            Alice          2
  3            Bob            1

------------------------------------------------------------------------
