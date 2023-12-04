# Course by sql
#sql #course

Вывод всех столбцов из таблицы
```sql
select * from employees;
```

Выбор определенных колонок из таблицы
```sql
select first_name last_name from employees;
```

### DISTINCT

Получение уникальных значений
```sql
select distinct job_id from job_history;
```

### Expressions in select list

```sql
select salary*2 form employees;
select salary*2+employee_id from employees;
select first_name, salary*2 from employees;
select salary, salary*2, salary+2-1 from employees;
select start_date, end_date, (end_date-start_date)+1 from employees;
```

Concatenate
```sql
select first_name||last_name from employees; 
select 'My name '||first_name||' '||last_name from employees; 
```
Alias
```sql
select first_name||last_name as name from employees; 

select first_name||last_name as "My name" from employees; 
```
*name* - это alias

#### Where
```sql
select * from employees where sapary = 1700;
select first_name from employees where first_name = 'Steaven';
select * from employees where sapary > 1700;
select first_name from employees where first_name != 'Steaven';
```

### Between
```sql
select * from employees where sapary between 1700 and 1800;
```

### IN
```sql
select * from employees where sapary in (1700, 1800);
```

### OR, AND
```sql
select * from employees where sapary=1700 and sapary=1800;
select * from employees where sapary=1700 or sapary=1800;
```

### IS NULL
```sql
select * from employees where sapary is null;
```

### LIKE
```sql
select first_name from employees where sapary like 'Sti%';
Stiven
Stiv
Stiv
select first_name from employees where sapary like '%v';
gorinv
pfv
select first_name from employees where sapary like 'S_V';
StV
SAV
select first_name from employees where sapary like 'S\_V' escape '\';
```

### NOT
```sql
select * from employees where not sapary=1700;
select * from employees where sapary not is null;
```

### ORDER BY

```sql
select * from employees order by salary;
select * from employees where salary<2000 order by salary;
select * from employees order by salary asc;
select * from employees order by salary desc;
select * from employees order by salary desc nulls first;
select * from employees order by salary desc nulls last;
```

## Функции

### Lower
```sql
select first_name, lower(first_neme) from employees;
select * from employees where lower(first_name) = 'vilem';
```

### Upper
```sql
select first_name,upper(first_neme) from employees;
select * from employees where upper(first_name) = 'vilem';
```

### Initcap
Первая  буква заглавная а остальные в нижнем регистре
```sql
select first_name,initcap(first_neme) from employees;
select * from employees where initcap(first_name) = 'Vilem';
```

### Cancat
```sql
select concat(first_neme, last_name) from employees;
```

### Length
```sql
select first_neme, length(first_name) from employees;
```

### Lpad(s,n,p), Rpad(s,n,p)
```sql
select lpad('Zaur', 3, '#') from dual;
Zaur###

select rpad('Zaur', 3, '#') from dual;
###Zaur
```

### Trim
```sql
select trim(trailing 'q' from 'Zaurqqqqq') from dual;
Zaur

select trim(liding 'q' from 'qqqqqZaur') from dual;
Zaur

select trim(both 'q' from 'qqqqqZaurqqqqq') from dual;
Zaur
```

### Instr
```sql
select first_neme from employees where instr(job_id, 'PROG')=4;

select first_neme from employees where instr(job_id, 'PROG', 4);
```

### Substr
```sql
select first_neme, substr(first_name, 4) from employees;
```

### Replace
```sql
select replace('Privet kak dela', '0', '***') from dual;
```

### Round
```sql
select round(3.14) from dual;
3

select round(3.14, 1) from dual;
3.1
```

### Trunc
```sql
select trunc(3.14) from dual;
3

select trunc(3.14, 1) from dual;
3.1
```



### Mod 
Остаток от деления
```sql
select mod(7, 3) from dual;
1

select mod(3, 9) from dual;
3
```

### SYSDATE 
```sql
select sysdate from dual;
20.10.95
```

### Month_between
```sql
select employee_id, months_between(end_date, start_date) from employee;
```

### TO_CHAR
```sql
select to_char(8) from dual;

select to_char(888888, '$9.999.99') from dual;
$8.888.88

```