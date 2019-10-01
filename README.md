
# Join Statements - Lab

## Introduction

In this lab, you'll practice your knowledge of `JOIN` statements, using various types of joins and various methods for specifying the links between them.

## Objectives

You will be able to:
- Write queries that make use of various types of Joins
- Join tables using foreign keys

## CRM Schema

In almost all cases, rather than just working with a single table you will typically need data from multiple tables. 
Doing this requires the use of **joins** using shared columns from the two tables. 

In this lab, you'll use the same Customer Relationship Management (CRM) database that you saw from the previous lesson.
<img src='images/Database-Schema.png' width="600">

## Connecting to the Database
Import the necessary packages and connect to the database **data.sqlite**.


```python
#Your code here
import sqlite3
import sqlite3
import pandas as pd
conn = sqlite3.connect('data.sqlite')
cur = conn.cursor()
```

## Display the names of all the employees in Boston.
Hint: join the employees and offices tables.


```python
#Your code here
cur.execute("""SELECT firstname, lastname from employees""").fetchall()
```




    [('Diane', 'Murphy'),
     ('Mary', 'Patterson'),
     ('Jeff', 'Firrelli'),
     ('William', 'Patterson'),
     ('Gerard', 'Bondur'),
     ('Anthony', 'Bow'),
     ('Leslie', 'Jennings'),
     ('Leslie', 'Thompson'),
     ('Julie', 'Firrelli'),
     ('Steve', 'Patterson'),
     ('Foon Yue', 'Tseng'),
     ('George', 'Vanauf'),
     ('Loui', 'Bondur'),
     ('Gerard', 'Hernandez'),
     ('Pamela', 'Castillo'),
     ('Larry', 'Bott'),
     ('Barry', 'Jones'),
     ('Andy', 'Fixter'),
     ('Peter', 'Marsh'),
     ('Tom', 'King'),
     ('Mami', 'Nishi'),
     ('Yoshimi', 'Kato'),
     ('Martin', 'Gerard')]



## Are there any offices that have zero employees?
Hint: Combine the employees and offices tables and use a group by.


```python
#Your code here
cur.execute("""SELECT * 
               FROM employees e
               left JOIN offices o
               using(officeCode)

               """)
df = pd.DataFrame(cur.fetchall())
df.columns = [i[0] for i in cur.description]
print('Who many empty offices ',len(df[df.officeCode.isnull()].head()))
df[['city', 'officeCode']].groupby('city').count()
```

    Who many emptyy offices  0
    




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>officeCode</th>
    </tr>
    <tr>
      <th>city</th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Boston</th>
      <td>2</td>
    </tr>
    <tr>
      <th>London</th>
      <td>2</td>
    </tr>
    <tr>
      <th>NYC</th>
      <td>2</td>
    </tr>
    <tr>
      <th>Paris</th>
      <td>5</td>
    </tr>
    <tr>
      <th>San Francisco</th>
      <td>6</td>
    </tr>
    <tr>
      <th>Sydney</th>
      <td>4</td>
    </tr>
    <tr>
      <th>Tokyo</th>
      <td>2</td>
    </tr>
  </tbody>
</table>
</div>



## Write 3 Questions of your own and answer them


```python
# Answers will vary
# What is the average number payment orders?
cur.execute("""select *
                From payments 
                left join orders
                using(customerNumber)
                """)
df = pd.DataFrame(cur.fetchall())
df.columns = [i[0] for i in cur.description]
df[['orderNumber', 'amount']].groupby('orderNumber').count().mean()

```




    amount    3.917178
    dtype: float64




```python
# Your code here Which customer has paid the most dollars?
cur.execute("""select *, sum(amount) as total
                From payments 
                left join customers
                using(customerNumber)
                group by customerName
                order by total desc""")
df = pd.DataFrame(cur.fetchall())
df.columns = [i[0] for i in cur.description]
df.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>customerNumber</th>
      <th>checkNumber</th>
      <th>paymentDate</th>
      <th>amount</th>
      <th>customerName</th>
      <th>contactLastName</th>
      <th>contactFirstName</th>
      <th>phone</th>
      <th>addressLine1</th>
      <th>addressLine2</th>
      <th>city</th>
      <th>state</th>
      <th>postalCode</th>
      <th>country</th>
      <th>salesRepEmployeeNumber</th>
      <th>creditLimit</th>
      <th>total</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>141</td>
      <td>NU627706</td>
      <td>2004-05-17</td>
      <td>26155.91</td>
      <td>Euro+ Shopping Channel</td>
      <td>Freyre</td>
      <td>Diego</td>
      <td>(91) 555 94 44</td>
      <td>C/ Moralzarzal, 86</td>
      <td></td>
      <td>Madrid</td>
      <td></td>
      <td>28034</td>
      <td>Spain</td>
      <td>1370</td>
      <td>227600.00</td>
      <td>715738.98</td>
    </tr>
    <tr>
      <th>1</th>
      <td>124</td>
      <td>NT141748</td>
      <td>2003-11-25</td>
      <td>45084.38</td>
      <td>Mini Gifts Distributors Ltd.</td>
      <td>Nelson</td>
      <td>Susan</td>
      <td>4155551450</td>
      <td>5677 Strong St.</td>
      <td></td>
      <td>San Rafael</td>
      <td>CA</td>
      <td>97562</td>
      <td>USA</td>
      <td>1165</td>
      <td>210500.00</td>
      <td>584188.24</td>
    </tr>
    <tr>
      <th>2</th>
      <td>114</td>
      <td>NR27552</td>
      <td>2004-03-10</td>
      <td>44894.74</td>
      <td>Australian Collectors, Co.</td>
      <td>Ferguson</td>
      <td>Peter</td>
      <td>03 9520 4555</td>
      <td>636 St Kilda Road</td>
      <td>Level 3</td>
      <td>Melbourne</td>
      <td>Victoria</td>
      <td>3004</td>
      <td>Australia</td>
      <td>1611</td>
      <td>117300.00</td>
      <td>180585.07</td>
    </tr>
    <tr>
      <th>3</th>
      <td>151</td>
      <td>KI884577</td>
      <td>2004-12-14</td>
      <td>39964.63</td>
      <td>Muscle Machine Inc</td>
      <td>Young</td>
      <td>Jeff</td>
      <td>2125557413</td>
      <td>4092 Furth Circle</td>
      <td>Suite 400</td>
      <td>NYC</td>
      <td>NY</td>
      <td>10022</td>
      <td>USA</td>
      <td>1286</td>
      <td>138500.00</td>
      <td>177913.95</td>
    </tr>
    <tr>
      <th>4</th>
      <td>148</td>
      <td>ME497970</td>
      <td>2005-03-27</td>
      <td>3516.04</td>
      <td>Dragon Souveniers, Ltd.</td>
      <td>Natividad</td>
      <td>Eric</td>
      <td>+65 221 7555</td>
      <td>Bronz Sok.</td>
      <td>Bronz Apt. 3/6 Tesvikiye</td>
      <td>Singapore</td>
      <td></td>
      <td>079903</td>
      <td>Singapore</td>
      <td>1621</td>
      <td>103800.00</td>
      <td>156251.03</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Your code here
```


```python
# Your code here
```

## Level Up: Display the names of every individual product that each employee has sold


```python
# Your code here
cur.execute("""select firstName, lastName, productName
                From employees
                join customers
                on employees.employeeNumber  = customers.salesRepEmployeeNumber
                join orders
                using(customerNumber)
                join orderdetails
                using(orderNumber)
                join products
                using(productcode)
                order by productName""")
df = pd.DataFrame(cur.fetchall())
df.columns = [i[0] for i in cur.description]
df.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>firstName</th>
      <th>lastName</th>
      <th>productName</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Leslie</td>
      <td>Jennings</td>
      <td>18th Century Vintage Horse Carriage</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Leslie</td>
      <td>Jennings</td>
      <td>18th Century Vintage Horse Carriage</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Leslie</td>
      <td>Jennings</td>
      <td>18th Century Vintage Horse Carriage</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Leslie</td>
      <td>Jennings</td>
      <td>18th Century Vintage Horse Carriage</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Leslie</td>
      <td>Jennings</td>
      <td>18th Century Vintage Horse Carriage</td>
    </tr>
  </tbody>
</table>
</div>



## Level Up: Display the Number of Products each employee has sold


```python
# Your code here
cur.execute("""select firstName, lastName, count(productcode) as numberProducts
                From employees
                join customers
                on employees.employeeNumber  = customers.salesRepEmployeeNumber
                join orders
                using(customerNumber)
                join orderdetails
                using(orderNumber)
                group by lastName""")
df = pd.DataFrame(cur.fetchall())
df.columns = [i[0] for i in cur.description]
df
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>firstName</th>
      <th>lastName</th>
      <th>numberProducts</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Loui</td>
      <td>Bondur</td>
      <td>177</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Larry</td>
      <td>Bott</td>
      <td>236</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Pamela</td>
      <td>Castillo</td>
      <td>272</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Julie</td>
      <td>Firrelli</td>
      <td>124</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Andy</td>
      <td>Fixter</td>
      <td>185</td>
    </tr>
    <tr>
      <th>5</th>
      <td>Martin</td>
      <td>Gerard</td>
      <td>114</td>
    </tr>
    <tr>
      <th>6</th>
      <td>Gerard</td>
      <td>Hernandez</td>
      <td>396</td>
    </tr>
    <tr>
      <th>7</th>
      <td>Leslie</td>
      <td>Jennings</td>
      <td>331</td>
    </tr>
    <tr>
      <th>8</th>
      <td>Barry</td>
      <td>Jones</td>
      <td>220</td>
    </tr>
    <tr>
      <th>9</th>
      <td>Peter</td>
      <td>Marsh</td>
      <td>185</td>
    </tr>
    <tr>
      <th>10</th>
      <td>Mami</td>
      <td>Nishi</td>
      <td>137</td>
    </tr>
    <tr>
      <th>11</th>
      <td>Steve</td>
      <td>Patterson</td>
      <td>152</td>
    </tr>
    <tr>
      <th>12</th>
      <td>Leslie</td>
      <td>Thompson</td>
      <td>114</td>
    </tr>
    <tr>
      <th>13</th>
      <td>Foon Yue</td>
      <td>Tseng</td>
      <td>142</td>
    </tr>
    <tr>
      <th>14</th>
      <td>George</td>
      <td>Vanauf</td>
      <td>211</td>
    </tr>
  </tbody>
</table>
</div>



## Summary

Congrats! You now know how to use join statements, along with leveraging your foreign keys knowledge!
