# SQL Query
In this section, we will learn how to control the results output of any SELECT query.
##Sorting Results
We are going to use the next database:<br />
<img src="https://github.com/alejandro-pentest/Hacking-Web/assets/161533623/fa658029-64df-44b1-bdcc-c3dfc7ed828e" width="350">



### ORDER BY
We can sort the results of any query using ORDER BY and specifying the column to sort by:
```sql
SELECT * FROM logins ORDER BY password;
```

<img src="https://github.com/alejandro-pentest/Hacking-Web/assets/161533623/7dbaabbb-60c3-465e-a88e-9968693c7111" width="350">


By default, the sort is done in ascending order, but we can also sort the results by `ASC` or `DESC`:
```sql
SELECT * FROM logins ORDER BY password DESC;
```
<img src="https://github.com/alejandro-pentest/Hacking-Web/assets/161533623/4a9ee588-09f2-4d14-a0c8-bf67e688a5a6" width="350">


It is also possible to sort by multiple columns, to have a secondary sort for duplicate values in one column:
```sql
SELECT * FROM logins ORDER BY password DESC, id ASC;
```
<img src="https://github.com/alejandro-pentest/Hacking-Web/assets/161533623/390c51aa-2ea7-4755-95c7-473a584e0197" width="350">
