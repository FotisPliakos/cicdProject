Architecture!

![image](https://github.com/FotisPliakos/cicdProject/assets/48320291/3c5b117b-f247-499f-b04e-9e9d7d83f8c8)

![image](https://github.com/FotisPliakos/cicdProject/assets/48320291/2681e11c-ba56-42cb-9088-b7dd8ab399d1)


######### Prerequisites

- JDK 1.8 or later
- Maven 3 or later
- MySQL 5.6 or later

### Technologies

- Spring MVC
- Spring Security
- Spring Data JPA
- Maven
- JSP
- MySQL

### Database

Here,we used Mysql DB
MSQL DB Installation Steps for Linux ubuntu 14.04:

- $ sudo apt-get update
- $ sudo apt-get install mysql-server

Then look for the file :

- /src/main/resources/accountsdb
- accountsdb.sql file is a mysql dump file.we have to import this dump to mysql db server
- > mysql -u <user_name> -p accounts < accountsdb.sql
