# Pila-Lamp-en-dos-niveles

## Tabla de Contenidos
- [Descripción](#descripción)
- [Vagrantfile](#vagrantfile)
- [Apache_prov](#apache)
- [Mysql_prov](#mysql)

## Descripción
En este proyecto estamos creado una aplicacion web LAMP en 2 niveles, una de las maquinas se le instalará Apache2 y en la otra el Mysql-server.
La máquina del Apache se encargará de gestionar las particiones web, mientras que el del Mysql se encargará de gestionar la base de datos.

## Vagrantfile
![image](https://github.com/user-attachments/assets/083ae84c-4b71-4c12-ad26-88f59cbf59de)

Aquí tenemos la creación de las 2 máquinas vituales:  
La del Apache tiene una red pública y otra privada (192.168.1.1), tambien tiene un reenvio de puertos del 80 al 8080 y por último tiene el aprovisionamiento.  
La del Mysql tiene una sola red privada (192.168.1.2) y cuenta tambien con el aprovisionamineto.  

## Apache_prov
Explicaremos que hace cada comando parte por parte.

![image](https://github.com/user-attachments/assets/d7f927dd-7d65-4488-9c65-1b815252aa00)

**Sleep 5:** Hace que se quede 5 segundos "dormido".  
**sudo apt update -y:** Actualiza la lista de paquetes.  
**sudo apt install -y apache2 php libapache2-mod-php php-mysql git:** Instala los programas mencionados.  
**git clone https://github.com/josejuansanchez/iaw-practica-lamp.git /var/www/kilianLAMP:** Descarga de ese enlace un archivo .zip y lo manda a esa ruta.  

![image](https://github.com/user-attachments/assets/da9749f7-0789-4725-a61f-907684947d32)

***sudo mv /var/www/kilianLAMP/src/* /var/www/kilianLAMP/:** Mueve todo lo que hay en esa ruta a la otra.  
**sudo chown -R www-data.www-data /var/www/kilianLAMP/:** Le cambia el propietario de a esa ruta.  
**sudo cp /etc/apache2/sites-available/000-default.conf /etc/apache2/sites-available/kilianLAMP.conf:** Hace una copia de ese fichero con otro nombre.  
**sudo sed -i 's|DocumentRoot /var/www/html|DocumentRoot /var/www/kilianLAMP|' /etc/apache2/sites-available/kilianLAMP.conf:** Cambia una linea por otra dentro del fichero mencionado.  

![image](https://github.com/user-attachments/assets/bd58912c-98ef-4e68-bba6-28adf92bbeb2)

**sudo a2ensite kilianLAMP.conf:** Activa el sitio web.  
**sudo a2dissite 000-default.conf:** Desactiva el sitio web.  

![image](https://github.com/user-attachments/assets/9716f8a9-a205-4d0a-b785-24c899740780)

**sudo cat <<EOL > /var/www/kilianLAMP/config.php:** Meterá las siguientes lineas de comando dentro del fichero.  
**<?php  
define('DB_HOST', '192.168.1.2');  
define('DB_NAME', 'lamp_db');  
define('DB_USER', 'kilian');  
define('DB_PASSWORD', '1234');  
\$mysqli = mysqli_connect(DB_HOST, DB_USER, DB_PASSWORD, DB_NAME);  
?>**  
**EOL**  
  
## Mysql_prov
Explicaremos que hace cada comando parte por parte.  

![image](https://github.com/user-attachments/assets/dcc55844-ef46-4ee4-8c4d-8691fc8bf9c3)

**sudo apt update:** Actualiza la lista de paquetes.  
**sudo apt install -y mysql-server git:** Instala los siguientes programas.  
**sudo systemctl start mysql:** Inicia el mysql.  
**sudo apt-get install net-tools:** Instala el siguiente programa.  
**sudo git clone https://github.com/josejuansanchez/iaw-practica-lamp.git:** Descarga del enlace un archivo .zip.  

![image](https://github.com/user-attachments/assets/87d7ca58-ea86-4fc3-92f2-5c61f0350861)

**sudo su:** Inicia como el root  
**mysql -u root < /home/vagrant/iaw-practica-lamp/db/database.sql:** Mete ka base de datos que se ha descargado en el mysql.  
**mysql -u root <<EOF** Ejecutará los siguientes comandos dentro de mysql.  
**CREATE USER 'kilian'@'192.168.1.1' IDENTIFIED BY '1234';**  
***GRANT ALL PRIVILEGES ON lamp_db.* TO 'kilian'@'192.168.1.1';**  
**FLUSH PRIVILEGES;**  
**EOF**  


