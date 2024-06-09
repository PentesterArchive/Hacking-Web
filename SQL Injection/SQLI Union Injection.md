# SQLI Union Injection.
As allways the first step must be to find a vulnerable fild, we can do it by injecting some symbols like `'` and seeing if we are getting an error.


<p align="center">
  <img src="https://github.com/alejandro-pentest/Hacking-Web/assets/161533623/589443a1-140d-4464-848e-05b6f48df448" width="700" alt="Sublime's custom image"/>
</p>

When we know that the field is vulnerable we need to know how many columns does the database has, we have two different methods to do it, we can use [ORDER BY](#order-by) or [UNION](#union).

## Getting the columns number.
The first step we must follow is knowing how many columns does the database have. We can do it using two different methods:
### ORDER BY.
Make a series of requests supplying a numeric value in the parameter, starting with the number 1 and incrementing it with each subsequent request.
If changing the number in the input affects the ordering of the results, the input is probably being inserted in to an `ORDER BY` clause.
Increasing this number to 2 should then change the display order of data to order by the second column.
> If the number supplied is therefore greater than the number of columns in the results set the query fails.


```sql
' ORDER BY 2 -- -
```
> RESULT: The query doesn't fail, meaning that the database has 2 or more columns

<p align="center">
  <img src="https://github.com/alejandro-pentest/Hacking-Web/assets/161533623/7bb06ecd-56de-44f2-99a8-a6f532bae657" width="500" alt="Sublime's custom image"/>
</p>


```sql
' ORDER BY 4 -- -
```
> RESULT: The query doesn't fail, meaning that the database has 4 or more columns

<p align="center">
  <img src="https://github.com/alejandro-pentest/Hacking-Web/assets/161533623/ede79664-2446-432a-b5aa-25d3ad52ad92" width="550" alt="Sublime's custom image"/>
</p>

> RESULT: The query fails, meaning that the database has less than 5 columns

```sql
' ORDER BY 5 -- -
```
<p align="center">
  <img src="https://github.com/alejandro-pentest/Hacking-Web/assets/161533623/e1b374f8-e691-4333-a5dd-cadd2400c151" width="550" alt="Sublime's custom image"/>
</p>




## UNION.
The other method is to attempt a Union injection with a different number of columns until we successfully get the results back. The first method [ORDER BY](#order-by) always returns the results until 
we hit an error, while __this method always gives an error until we get a success__.

```sql
' UNION SELECT 1 -- -
```
> RESULT: We get an error because the database has't got 1 column.
<p align="center">
  <img src="https://github.com/alejandro-pentest/Hacking-Web/assets/161533623/0e3b8b42-d50e-4400-9271-c135afb52f71" width="550" alt="Sublime's custom image"/>
</p>

```sql
' UNION SELECT 1, 2, 3, 4 -- -
```
> RESULT: We don't get an error because the database has EXACTLY 4 columns.
<p align="center">
  <img src="https://github.com/alejandro-pentest/Hacking-Web/assets/161533623/55c69c71-865c-4449-99a1-13ea450696c9" width="550" alt="Sublime's custom image"/>
</p>


```sql
' UNION SELECT 1, 2, 3, 4, 5 -- -
```
> RESULT: We get an error because the database hasn't got 5 columns.
<p align="center">
  <img src="https://github.com/alejandro-pentest/Hacking-Web/assets/161533623/ecd7f0b0-6fe8-4876-9920-47335155ce83" width="550" alt="Sublime's custom image"/>
</p>

-------------------

### Location of Injection

While __a query may return multiple columns, the web application may only display some of them__. So, __if we inject our query in a column that is not printed on the page, 
we will not get its output__. This is why we need to determine which columns are printed to the page, to determine where to place our injection. In the previous example, 
while the injected query returned 1, 2, 3, and 4, we saw only 2, 3, and 4 displayed back to us on the page as the output data. <br />
It is very common that not every column will be displayed back to the user. For example, the ID field is often used to link different tables together, but the user doesn't need to see it. 
> This tells us that columns 2 and 3, and 4 are printed to place our injection in any of them. We cannot place our injection at the beginning, or its output will not be printed.

To test that we can get actual data from the database 'rather than just numbers,' we can use the `@@version` SQL query as a test and place it in an outputed column and see the result:
```sql
' UNION SELECT 1, 2, @@version, 4 -- -
```

<p align="center">
  <img src="https://github.com/alejandro-pentest/Hacking-Web/assets/161533623/516f5457-4c22-42b6-9913-8cab18362c40" width="700" alt="Sublime's custom image"/>
</p>


We can see the user too.
```sql
' UNION SELECT 1, 2, user() , 4 -- -
```

<p align="center">
  <img src="https://github.com/alejandro-pentest/Hacking-Web/assets/161533623/e750c420-1297-4d28-bf44-aeb6e1f61b2f" width="700" alt="Sublime's custom image"/>
</p>














