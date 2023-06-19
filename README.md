Architecture!

![image](https://github.com/FotisPliakos/cicdProject/assets/48320291/c1d79144-e0bd-47d3-9e1e-4d67518a699d)
![image](https://github.com/FotisPliakos/cicdProject/assets/48320291/9c32d8cc-1ee6-45d6-b163-af38ef9ab101)


### Prerequisites

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
