## PostgreSQL installation with Docker

- First, install Docker Desktop (and make sure it's running) and open PowerShell.


- Run the following command. **-e** is used to set up administrator password. This is the password you use to log in to the server. Then, we can specify port using **-p**. The first number is used for external port to connect to the server. The second is for the port inside of the container. You can use different number for the first number but the second number will be always **5432**. **-d** means the version of postgres we want inside of the container.  


```
docker run --name postgresql-server -e POSTGRES_PASSWORD=yejin123456 -p 5432:5432 -d postgres:16.2
```


![postgre-server-installation](/pictures/PostgreSQL/installation/postgre-server-installation.PNG "postgresql server installation")


- When you are done, you can close your Powershell window and go back to Docker desktop. You can see a new container is generated in your dashboard.


![new-container-dashboard](/pictures/PostgreSQL/installation/new-container-dashboard.PNG "new container dashboard")


- As long as you see the Postgre server status as **running**, the server will be available with the pgAdmin client application. 


- To connect to the postgre server, run the command:


```
docker exec -it postgresql-server psql -U postgres
```


![connect-to-server](/pictures/PostgreSQL/installation/connect-to-server.PNG "connect to postgresql server")


![experiment](/pictures/PostgreSQL/installation/experiment.PNG "experiment")


- Let's create a database called colors:


```
CREATE DATABASE colors;
```


- Remember, we are using **psql** utility and it also supports a number of command shortcuts. That all starts with backslash (\). For instance, to see all of the databases available on the server, type backslash(\) + lowercase l.


![backslash-l](/pictures/PostgreSQL/installation/backslash-l.PNG "backslash l")


- In order to use a database, you need to switch the context into it. We are now at the user postgres, but if we run:


```
\c colors
```


- You are now connected to database colors. At this point, we can add a table to the database.


```
CREATE TABLE colors (ColorID int, ColorName char(20) );
```


![create-table](/pictures/PostgreSQL/installation/create-table.PNG "create table")


- Now, insert values into the table:


```
INSERT INTO colors VALUES (1, 'red'), (2, 'blue'), (3, 'green'), (4, 'yellow'), (5, 'pink'), (6, 'purple'), (7, 'orange'), (8, 'brown')
```


- Finally, we can ask the server what data is stored in the table:


```
SELECT * FROM colors
```


![insert-values-query-data](/pictures/PostgreSQL/installation/insert-values-and-see-data.PNG "insert values and query data")



- pgAdmin is a graphical interface that allows us to connect to lots of database servers at the same time.


![pgadmin](/pictures/PostgreSQL/installation/pdadmin.PNG "pgadmin")


- As we installed pgAdmin as a standalone software, we need to register the server manually. Right click the Servers and Register > Server.


![register-server](/pictures/PostgreSQL/installation/register-server.PNG "register server")


- Then, in the General tab, enter the server name and background color for your purposes (e.g. development, production...).


![name-and-color](/pictures/PostgreSQL/installation/name-and-color.PNG "name and color")


- Moving on to the Connection tab, enter your host name for which you can check by going back to Docker Desktop dashboard:


![localhost](/pictures/PostgreSQL/installation/localhost.png "localhost")


- Go back to your pgAdmin Register Server window and enter it accordingly. Then click save:


![fill-in-accordingly](/pictures/PostgreSQL/installation/fill-in-accordingly.PNG "fill in accordingly")


- Then connection should appear on the right side bar.


![connection-established](/pictures/PostgreSQL/installation/connection-established.PNG "connection established")


- You can see a treeview of database servers we are connected to:


![treeview](/pictures/PostgreSQL/installation/treeview.PNG "treeview")


- To take a quick look at the colors table, make sure you select the Tables > colors and On the top Browser menu click the table icon. 


![quick-table](/pictures/PostgreSQL/installation/quick-table.PNG "quick table")


- The filtered rows icon is for a quick search inside the table. Click the icon and run this:


![filtered-rows](/pictures/PostgreSQL/installation/filtered-rows.PNG "filtered rows")


![filtered-rows-run](/pictures/PostgreSQL/installation/filtered-rows-run.PNG "filtered rows run")


- The final icon on the Browser menu is **PSQL Tool**. The first time you run this tool, you may see a message to set the **binary path**. This just means that the pgAdmin needs to know where the PSQL application is on your computer in order to run it. In this case, find **psql.exe** file and find where it is (PATH) on your computer.


- You can check your binary path in File > Preferences > Path > Binary Path > PostgreSQL Binary Path. If you click validate and it shows your version correctly, then you are good to go.


![binary-path-check](/pictures/PostgreSQL/installation/binary-path-check.PNG "binary path check")


- You can create a database using pdAdmin GUI. Right click the PostSQL 16.2 which is at the very top and create database. You can give the database a name and that's it.


![create-database-gui](/pictures/PostgreSQL/installation/create-database-gui.PNG "create database gui")



