
- DESCRIBE my_table_name;
- SHOW COLUMNS FROM my_table_name;

위의 두 개의 SQL은 비슷한 결과를 표시한다.

```SQL

-- SQL 입력
DESCRIBE employees;

```

결과

| Field | Type | Null | Key | Default | Extra | 
|-----------|--------------|------|-----|---------|-------|
| id | int | NO | PRI | NULL | |
| name | varchar(255) | YES | | NULL | | 
| salary | int | YES | | 5000 | | 
| hire_date | date | YES | | NULL | |


- INFORMATION_SCHEMA.COLUMNS 

```SQL

-- SQL 입력

SELECT COLUMN_NAME, COLUMN_DEFAULT FROM INFORMATION_SCHEMA.COLUMNS WHERE TABLE_SCHEMA = 'company' AND TABLE_NAME = 'employees';

```

결과

| COLUMN_NAME | COLUMN_DEFAULT |
|-------------|----------------| 
| id | NULL | | name | NULL | 
| salary | 5000 |
| hire_date | NULL |