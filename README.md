# GeoServer
Web framework for GeoSolver

## Local server hosting: general instruction
1. Make sure you have Python 2 (tested on 2.7.6) and MySQL installed.
2. Clone this repository, as well as `geosolver` and `equationtree`. They have to be added to the `$PYTHONPATH`. 
3. Make sure the MySQL root acess credentials agrees with `DATABASES` in `GeoServer/geoserver/settings/local.py`.
4. Log in to MySQL server and create a database `geodb` in MySQL: 
  ```mysql
  create database geodb;
  ```
  
5. Install all required packages for the server by typing on the terminal: 
  ```bash
  pip install numpy scipy scikit-learn sympy networkx nltk inflect pyparsing matplotlib pydot2 mysql-python django django-picklefield jsonfield django-storages boto django-modeldict pillow unipath beautifulsoup4 requests
  ```
  
6. Install OpenCV 3 (tested on 3.0.0). Make sure python wrappers are accessible via `$PYTHONPATH`.
7. Change directory to `GeoServer/geoserver`. 
8. Type on the terminal: 
  ```bash
  python manage.py migrate --settings=geoserver.settings.local
  ```
  This will set up all tables in the `geodb` database.

9. To run the server, type on the terminal (in the same directory, `GeoServer/geoserver`): 
  ```bash
  python manage.py runserver --settings=geoserver.settings.local
  ```

  You should see something like `Starting development server at http://127.0.0.1:8000`.
  Note that if you want to make the server visible to other computers, you specify the ip address to be `0`, i.e. `python manage.py runserver 0:8080 --settings=geoserver.settings.local` (`8080` is port number of your choice).
  
10. Check if everything is good by accessing `http://localhost:8000/questions/list/all`. You should see a webpage with an empty table.

## Dumping data
Now that you have server running, you want to dump data on it (otherwise you will have to upload every question yourself!).

1. download these files: [media.tar.gz](https://drive.google.com/file/d/0B_NX3z_sIBWTel9sRUNmbWdvSzQ/view?usp=sharing), [db.json](https://drive.google.com/file/d/0B_NX3z_sIBWTc1kxcHAxb3I1X0E/view?usp=sharing)
2. Place `db.json` and unzipped media folder in `GeoServer/geoserver`.
3. To dump the data onto the database, run:
  ```bash
  python manage.py loaddata db.jon --settings=geoserver.settings.local
  ```
  
4. Now you should be able to see questions when accessing `http://localhost:8000/questions/list/all`.

## Ubuntu helps
* To meet the python and MySQL requirements, run:
  ```bash
  apt-get install python mysql-server libmysqlclient-dev
  ```
  
* If you are going to install numpy / scipy via pip, you want to install:
  ```bash
  apt-get install gfortran
  ```
  
* To log-in to MySQL, run:
  ```bash
  mysql -u root -p
  ```
  
* To change MySQL password:
  1. stop the mysql server: `sudo /etc/init.d/mysql stop`
  2. start the mysqld configuration: `sudo mysqld --skip-grant-tables &`
  3. log in to MySQL as root: `mysql -u root mysql`
  4. replace YOURNEWPASSWORD with your new password: `UPDATE user SET Password=PASSWORD('YOURNEWPASSWORD') WHERE User='root'; FLUSH PRIVILEGES; exit;`
  5. kill the temporary server: `sudo killall -9 mysqld ?`
  6. start the normal server: `sudo service mysql start`