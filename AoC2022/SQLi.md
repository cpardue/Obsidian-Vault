The Story

![AoC day 16 banner](https://tryhackme-images.s3.amazonaws.com/user-uploads/5ed5961c6276df568891c3ea/room-content/67234c28fa6771b3c6d6e39260be4c3e.png)

Check out InsiderPhD's video walkthrough for Day 16 [here](https://www.youtube.com/watch?v=iv02-Oi0TvM)!  

Set to have all their apps secured, the elves turned towards the one Santa uses to manage the present deliveries for Christmas. Elf McSkidy asked Elf Exploit and Elf Admin to assist you in clearing the application from SQL injections. When presented with the app's code, both elves looked a bit shocked, as none of them knew how to make any sense of it, let alone fix it. **"We used to have an Elf McCode, but he founded a startup and helps us no more"**, said Admin.

After a bit of talk, it was decided. The elves returned carrying a pointy hat and appointed you as the new Elf McCode. Congratulations on your promotion!

Deploying the Virtual Machine

During this task, you'll use a Virtual Machine (VM) where all the tools you need are already installed. The instructions on what is available and how to use it will be provided further ahead. Since the VM takes a couple of minutes to start, it might be a good idea to click the `Start Machine` button on the top-right corner of this task now. To access the contents in the machine, you will only need your browser (you won't need the AttackBox or VPN for this one).

SQL Refresher

**Structured Query Language (SQL)** is the traditional language used to ask databases for information. When you build any application that relies on a database, the app will need to create SQL sentences on the fly and send them to the database engine to retrieve the required information for your app to work. Luckily for you, SQL was built with simplicity in mind, and its syntax is supposed to resemble straightforward English sentences to make it easier for programmers to understand.

But before explaining the SQL syntax, let's talk about the specific database engine used on our app: MySQL. MySQL stores information in structures called tables. Think of them as any table in a spreadsheet document where you have **columns** and **rows**. For clarity, let's check one of the tables in use in the current application where the toys are stored:

![A database table](https://tryhackme-images.s3.amazonaws.com/user-uploads/5ed5961c6276df568891c3ea/room-content/bd97758d1b832dcbbaa1f8d68ae98650.png)  

As you can see, each row of the table corresponds to a different toy, and each column is a **field** of data that describes the toy. Looking at the third row, we can see that our toys table contains a toy named "Car" and that there are 12 units available for it. Pretty easy.  

As mentioned before, when an app needs to retrieve information from the database, it will need to build an **SQL query**. Queries are simple instructions that ask for specific data in a structured way that the database can understand. To query information, we will use **SELECT** statements, indicating which rows of which table we want to retrieve. If we wanted to get all of the columns from the "toys" table in our database, we could use the following statement:

```sql
SELECT * FROM toys;
```

![Simple select query](https://tryhackme-images.s3.amazonaws.com/user-uploads/5ed5961c6276df568891c3ea/room-content/b149b3ba14bf78a737f26772d0f8c7e5.png)  

Notice the asterisk(*), which indicates that you want to retrieve all columns from the table. If you need to ask for specific columns, you can replace the asterisk with a comma-separated list of columns. For example, to retrieve only the name and quantity columns, you can issue the following SQL query:

```sql
SELECT name, quantity FROM toys;
```

![select specific columns](https://tryhackme-images.s3.amazonaws.com/user-uploads/5ed5961c6276df568891c3ea/room-content/cea1fa400777acd81f95e5b95ebb6054.png)  

All table rows are returned in both cases before, but you can filter those if needed using a **WHERE** clause. Suppose you want to filter the results of the last query so that you only get the toys for which there is at least a quantity of 20. You could do so with the following statement:

```sql
SELECT name, quantity FROM toys WHERE quantity >= 20;
```

![Using WHERE filters](https://tryhackme-images.s3.amazonaws.com/user-uploads/5ed5961c6276df568891c3ea/room-content/bcf89fa6f3dd293c319d29f19bfdb694.png)  

In real-world apps, you are likely to find much more complex queries in some cases, but what we've covered so far should be enough for the rest of the room.

Sending SQL Queries from PHP

Now that we understand how a SELECT statement works, let's see how a PHP application builds and sends such a query to MySQL. Although we are focusing on PHP and MySQL, the same idea generally applies to other programming languages.

The first step is always to get a connection to the database from our code. To do so, PHP includes the **mysqli** extension, which provides the `mysqli_connect()` function. The function receives the IP or name of the database server as a first parameter (`$server`), followed by the username (`$user`) and password (`$pwd`), and finally, the name of the schema to use(`$schema`), which is just an identifier of the database to which we are connecting. As a result, the function returns a connection handler, which can be used to send further SQL Queries. Think of the connection handler as a variable that holds the connection information to the database, so that queries can be sent to it:

```php
$server="db";
$user="logistics_user";
$pwd="somePass123";
$schema="logistics";

$db=mysqli_connect($server,$user,$pwd,$schema);
```

Once the connection is made, we can issue SQL queries using the `mysqli_query()` function. The first parameter we pass to the function is the connection handler we got before, and the second parameter is a string with our SQL query:

```php
$query="select * from users where id=1";
$elves_rs=mysqli_query($db,$query);
```

As a result of executing the query, we obtain an SQL result set and store it in the `$elves_rs` variable in our example. A result set is nothing more than an object that contains the results of our query, which can be used in the rest of our program to access the resulting data.

As you can see, it is all as easy as building a string with our query and sending it to the database! 

Building Dynamic Websites

Now here's where things get interesting. If you check Santa's web application, you can access an elf's profile by using a URL like this:

[http://LAB_WEB_URL.p.thmlabs.com/webapp/elf.php?id=2](http://lab_web_url.p.thmlabs.com/webapp/elf.php?id=2)

![Elf McSkidy Profile](https://tryhackme-images.s3.amazonaws.com/user-uploads/5ed5961c6276df568891c3ea/room-content/14c35693f57e1001f03b84eaafab91c2.png)  

Depending on the number you put on the `id` parameter of the URL, you get served the profile of a different elf. Behind the curtains, this works by creating an SQL query that embeds the `id` parameter value and returns the information on the corresponding elf.

![Building dynamic queries](https://tryhackme-images.s3.amazonaws.com/user-uploads/5ed5961c6276df568891c3ea/room-content/a8f7cb56f722249c4e453f3de2c072c3.png)  

In code, it would look like this:

```php
$query="select * from users where id=".$_GET['id'];
$elves_rs=mysqli_query($db,$query);
```

The first line builds an SQL query by concatenating the `$_GET['id']` variable as part of the where clause. Note that in PHP, you can access any parameter in the URL as `$_GET['name_of_parameter']`. This query will ask the database for all columns of the table users that correspond to the elf with a specific id. The second line sends the query and returns the information of one particular elf as a result set that we store in the `$elves_rs` variable. The rest of the website then parses the result set and renders the page accordingly.

If you test the website, you can see that it works as expected. You have, however, introduced an SQL injection vulnerability in your code that could allow an attacker to dump your whole database!

SQL Injection (SQLi)

The problem with the method shown before is that it takes untrusted input from the user and concatenates it to an SQL query without any questions asked. As seen in the previous day's task, our app should thoroughly validate any input the user sends before using it. If it doesn't, unexpected things may happen.

In the case of SQL and our example, an attacker can send SQL syntax through one of the various parameters of the app's URLs, which might end up being concatenated to some SQL query in code, potentially changing its intended purpose.

Let's get back to the elf's profile page to understand this better. Remember the application is creating a query by concatenating whatever is sent in the `id` parameter as part of the WHERE clause:

```php
$query="select * from users where id=".$_GET['id'];
```

If the attacker sends the following through the id parameter of the URL:

[http://LAB_WEB_URL.p.thmlabs.com/webapp/elf.php?id=-1 OR id = 4](http://lab_web_url.p.thmlabs.com/webapp/elf.php?id=-1%20OR%20id%20=%204)

When PHP concatenates "-1 OR id = 4" to our SQL statement, it will end up with this query:

```sql
select * from users where id=-1 OR id = 4
```

Suddenly, the attacker has injected some SQL syntax that, when concatenated to our original query, ends up serving the data of the elf with `id=4` for some weird reason.

![SQL injection 101](https://tryhackme-images.s3.amazonaws.com/user-uploads/5ed5961c6276df568891c3ea/room-content/9172c99c0a762ac5500956cdf0f04629.png)  

If we read the resulting query string, we can see that our WHERE clause was modified to filter only the elves that either have `id=-1` or `id=4`. Since the id values used by the database are likely all positive numbers, no elf will match `id=-1`. Therefore, the database will only return the elf with `id=4`.

While this example shows a harmless injection, a skilled attacker can try to get your server to run much more complex SQL queries and potentially force the database to return any data on any table they want. Just as an example, look at what happens when you put the following in the `id` parameter:

[http://LAB_WEB_URL.p.thmlabs.com/webapp/elf.php?id=-1 union all select null,null,username,password,null,null,null from users](http://lab_web_url.p.thmlabs.com/webapp/elf.php?id=-1%20union%20all%20select%20null,null,username,password,null,null,null%20from%20users)

The SQL injected will make the database return all of the users and passwords of the application. Santa won't like this, for sure! If you are interested in learning more about SQL injection from an attacker's perspective, you can check the [SQL injection room](https://tryhackme.com/room/sqlinjectionlm). For the rest of this task, however, we will focus on the defensive side and look at ways to prevent SQL injections in our code, so don't worry too much if the above URL looks hard to understand.

Fixing the App With Elf Exploit and Elf Admin

Armed with all this knowledge, we are ready to fix the app. You'll have access to a simple editor to modify the app's source code during this task. Any changes you make in the editor will go live instantly as long as you save your changes (by pressing `CTRL+S`). If you are using the VPN connection, you can access the code editor from any browser of your preference. If you use the AttackBox instead, Firefox is installed and available on the machine's desktop. To get to the code editor, point your browser to the following address:

[http://LAB_WEB_URL.p.thmlabs.com/](http://lab_web_url.p.thmlabs.com/)

To enter the code editor, use the following credentials:

![THM key](https://tryhackme-images.s3.amazonaws.com/user-uploads/5ed5961c6276df568891c3ea/room-content/f8eb6e78ad80791f4c2bc42c421364ba.png)

**Username**

coder

**Password**

coder

In addition to the code editor, you will have access to a chat to communicate with Elf Exploit and Elf Admin. While they don't speak too much, you can request them to check the application for you. If you remember correctly, they don't know a thing about coding. However, Elf Exploit will help you identify parts of the app that are vulnerable to SQLi. Elf Admin, on the other hand, will check that the application is running as expected, so if you make a change that breaks the application somehow, he will let you know so you can roll back and try again. In combination, they will tell you if your changes solve vulnerabilities while avoiding altering how the app is supposed to work.

![Code Editor Layout](https://tryhackme-images.s3.amazonaws.com/user-uploads/5ed5961c6276df568891c3ea/room-content/d25e248ac1ab1791a6316451ce65ab2f.png)  

To ask the elves to do a recheck of the app, scroll down the elf chat to find the Run Checks button:

![Run Checks Button](https://tryhackme-images.s3.amazonaws.com/user-uploads/5ed5961c6276df568891c3ea/room-content/5d7f03c0848004a911875745dd0ca88e.png)  

Remember that you can always check the website after any changes by visiting the following link:

[http://LAB_WEB_URL.p.thmlabs.com/webapp/](http://lab_web_url.p.thmlabs.com/webapp/)

Now let's get to work!

Fixing SQLi by Data Type Validation

One of the easiest and most effective ways to prevent SQL injections is to ensure that any data that the user can manipulate that you are concatenating as part of an SQL statement is actually from the type you expect. Let's go to our elf chat and click the **Run Checks** button.

Elf Exploit should tell you that he successfully injected some SQL via the id parameter of `elf.php`. Let's open this file in our code editor and look at lines 4-5:

```php
$query="select * from users where id=".$_GET['id'];
$elves_rs=mysqli_query($db,$query);
```

The website takes the `id` parameter from the URL and concatenates it to an SQL query, as shown before.

We can reasonably assume that the website expects an integer `id` to be sent. To avoid injections, we can convert whatever the user inputs in the id parameter to an integer. For this purpose, we will be using the `intval()` function. This function will take a string and try to convert it into an integer. If no valid integer is found on the string, it will return 0, which is also an integer. To clarify this, let's look at some values and how they would be converted:

```php
intval("123") = 123
intval("tryhackme") = 0
intval("123tryhackme") = 123
```

Putting this to use, we can modify line 4 of `elf.php` to look like this:

```php
$query="select * from users where id=".intval($_GET['id']);
```

That way, even if the attacker sends an SQL injection payload via the id parameter, the app will try converting it to an integer before concatenating it as part of the SQL statement. In the worst-case scenario, the string gets converted to a 0, which is still harmless in this particular case.

Make sure to press `CTRL+S` to save your changes on the document, and ask the elves to recheck the app. This time Elf Exploit will again tell you he can inject SQL, but in a different way. This happens because the `id` parameter is used twice in `elf.php` to form two separate SQL queries: one to get the elf's information and another to get any toys built by them. Find where this happens and fix the vulnerability. Once you do, ask the elves to recheck, and if your fix is correct, you'll get the first flag.

Notice that for most data types, you will be able to make something similar. If you expect to receive a float number, you can use `floatval()` just the same. Even if values are not numeric but follow some specific structure, you could implement your own validators to ensure that data conforms with a given format. Think, for example, of a parameter used to send IP addresses. You could quickly implement a simple function to check if the IP is well formed and opt not to run the SQL query if it isn't.

Fixing SQLi Using Prepared Statements

While in some cases, you may secure your code with a simple validator, there are situations where you need to allow the user to pass arbitrary strings through a parameter. One example of this can be seen in the search bar of our application.

Every time a search is done, it gets sent to search-toys.php via the `q` parameter. If you ask the elves to recheck the application right now, Elf Exploit should have a way to take advantage of a vulnerability in that parameter. If we open `search-toys.php` in our code editor, we can quickly see that a query is built in lines 4-5:

```php
$query="select * from toys where name like '%".$_GET['q']."%' or description like '%".$_GET['q']."%'";
$toys_rs=mysqli_query($db,$query);
```

Here, the `q` parameter gets concatenated twice into the same SQL sentence. Notice that in both cases, the data in `q` is wrapped around single quotes, which is how you represent a string in SQL. The problem with having PHP build the query is that the database has no other option but to trust what it is being given. If an attacker somehow injects SQL, PHP will blindly concatenate the injected payload into the query string, and the database will execute it.

![Building SQL Query by Concatenation](https://tryhackme-images.s3.amazonaws.com/user-uploads/5ed5961c6276df568891c3ea/room-content/94464c2dd10b21b108fe9b5fa5ed24f0.png)  

While there are a couple of ways to go about this, the safest bet is to use prepared statements to prevent SQL injections.  

**Prepared statements** allow you to separate the syntax of your SQL sentence from the actual parameters used on your WHERE clause. Instead of building a single string by concatenation, you will first describe the structure of your SQL query and use placeholders to indicate the position of your query's parameters. You will then bind the parameters to the prepared statement in a separate function call.

![Building SQL Queries via Prepared Statements](https://tryhackme-images.s3.amazonaws.com/user-uploads/5ed5961c6276df568891c3ea/room-content/b03ab555f94e432238a260e3557fb492.png)  

Instead of providing a single SQL query string, we will send any dynamic parameters separately from the query itself, allowing the database to stick the pieces together securely without depending on PHP or the programmer. Let's see how this looks in the code.

First, we will modify our initial query by replacing any parameter with a placeholder indicated with a question mark (`?`). This will tell the database we want to run a query that takes two parameters as inputs. The query will then be passed to the `mysqli_prepare()` function instead of our usual `mysqli_query()`. `mysqli_prepare()` will not run the query yet but will indicate to the database to prepare the query with the given syntax. This function will return a prepared statement.

```php
$query="select * from toys where name like ? or description like ?";
$stmt = mysqli_prepare($db, $query);
```

To execute our query, MySQL needs to know the value to put on each placeholder we defined before. We can use the `mysqli_stmt_bind_param()` function to attach variables to each placeholder. This function requires you to send the following function parameters:

The first parameter should be a reference to the prepared statement to which to bind the variables. 

The second parameter is a string composed of one letter per placeholder to be bound, where letters indicate each variable's data type. Since we want to pass two strings, we put `"ss"` in the second parameter, where each "s" represents a string-typed variable. You can also use the letters "i" for integers or "d" for floats. You can check the full list in [PHP's documentation](https://www.php.net/manual/en/mysqli-stmt.bind-param.php).

After that, you will need to pass the variables themselves. You must add as many variables as placeholders defined with `?` in your query, which in our case, are two. Notice that, in our example, both parameters have the same content, but in other cases, it may not be so.

The resulting code for this would be as follows:

```php
$q = "%".$_GET['q']."%";
mysqli_stmt_bind_param($stmt, 'ss', $q, $q);
```

Once we have created a statement and bound the required parameters, we will execute the prepared statement using `mysqli_stmt_execute()`, which receives the statement `$stmt` as its only parameter.

```php
mysqli_stmt_execute($stmt);
```

Finally, when a statement has been executed, we can retrieve the corresponding result set using the `mysqli_stmt_get_result()`, passing the statement as the only parameter. We'll assign the result set to the `$toys_rs` variable as in the original code.

```php
$toys_rs=mysqli_stmt_get_result($stmt);
```

Our final resulting code should look like this:

```php
$q = "%".$_GET['q']."%";
$query="select * from toys where name like ? or description like ?";
$stmt = mysqli_prepare($db, $query);
mysqli_stmt_bind_param($stmt, 'ss', $q, $q);
mysqli_stmt_execute($stmt);
$toys_rs=mysqli_stmt_get_result($stmt);
```

Be sure to save your changes by using `CTRL+S`. If you ask the elves to recheck the app, they should now tell you the vulnerability has been fixed and give you the second flag.