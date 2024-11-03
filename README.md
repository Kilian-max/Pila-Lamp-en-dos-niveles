# Pila-Lamp-en-dos-niveles

## Tabla de Contenidos
- [Descripción](#descripción)
- [Vagrantfile](#vagrantfile)
- [Apache_prov](#apache_prov)
- [Mysql_prov](#mysql_prov)
- [Resultado](#resultado)

## Descripción
En este proyecto estamos creando una aplicación web LAMP en dos niveles. Una de las máquinas tendrá instalado Apache2 y la otra tendrá instalado MySQL Server.  
La máquina de Apache se encargará de gestionar las particiones web, mientras que la de MySQL se encargará de gestionar la base de datos.  

## Vagrantfile
![image](https://github.com/user-attachments/assets/083ae84c-4b71-4c12-ad26-88f59cbf59de)

Aquí tenemos la creación de las dos máquinas virtuales:  
La del Apache tiene una red pública y otra privada (192.168.1.1), también tiene un reenvío de puertos del 80 al 8080 y, por último, tiene el aprovisionamiento.  
La de MySQL tiene una sola red privada (192.168.1.2) y cuenta también con el aprovisionamiento.

## Apache_prov
Explicaremos que hace cada comando parte por parte.

![image](https://github.com/user-attachments/assets/d7f927dd-7d65-4488-9c65-1b815252aa00)

**Sleep 5:** Hace que se quede 5 segundos "dormido".  
**Sudo apt update -y:** Actualiza la lista de paquetes.  
**Sudo apt install -y apache2 php libapache2-mod-php php-mysql git:** Instala los programas mencionados.  
**Git clone https://github.com/josejuansanchez/iaw-practica-lamp.git /var/www/kilianLAMP:** Descarga de ese enlace un archivo .zip y lo manda a esa ruta.  

![image](https://github.com/user-attachments/assets/da9749f7-0789-4725-a61f-907684947d32)

***Sudo mv /var/www/kilianLAMP/src/* /var/www/kilianLAMP/:** Mueve todo lo que hay en esa ruta a la otra.  
**Sudo chown -R www-data.www-data /var/www/kilianLAMP/:** Le cambia el propietario de a esa ruta.  
**Sudo cp /etc/apache2/sites-available/000-default.conf /etc/apache2/sites-available/kilianLAMP.conf:** Hace una copia de ese fichero con otro nombre.  
**Sudo sed -i 's|DocumentRoot /var/www/html|DocumentRoot /var/www/kilianLAMP|' /etc/apache2/sites-available/kilianLAMP.conf:** Cambia una linea por otra dentro del fichero mencionado.  

![image](https://github.com/user-attachments/assets/bd58912c-98ef-4e68-bba6-28adf92bbeb2)

**Sudo a2ensite kilianLAMP.conf:** Activa el sitio web.  
**Sudo a2dissite 000-default.conf:** Desactiva el sitio web.  

![image](https://github.com/user-attachments/assets/9716f8a9-a205-4d0a-b785-24c899740780)

**Sudo cat <<EOL > /var/www/kilianLAMP/config.php:** Meterá las siguientes lineas de comando dentro del fichero.  
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

**Sudo apt update:** Actualiza la lista de paquetes.  
**Sudo apt install -y mysql-server git:** Instala los siguientes programas.  
**Sudo systemctl start mysql:** Inicia el mysql.  
**Sudo apt-get install net-tools:** Instala el siguiente programa.  
**Sudo git clone https://github.com/josejuansanchez/iaw-practica-lamp.git:** Descarga del enlace un archivo .zip.  

![image](https://github.com/user-attachments/assets/87d7ca58-ea86-4fc3-92f2-5c61f0350861)

**Sudo su:** Inicia como el root  
**Mysql -u root < /home/vagrant/iaw-practica-lamp/db/database.sql:** Mete la base de datos que se ha descargado en el MySQL.  
**mMysql -u root <<EOF** Ejecutará los siguientes comandos dentro de MySQL.  
**CREATE USER 'kilian'@'192.168.1.1' IDENTIFIED BY '1234';**  
***GRANT ALL PRIVILEGES ON lamp_db.* TO 'kilian'@'192.168.1.1';**  
**FLUSH PRIVILEGES;**
**EOF**  

## Resultado
Una vez levantamos todas las máquinas (vagrant up --provision), ponemos en el buscador localhost:8080 y nos saldrá lo siguiente.

![image](https://github.com/user-attachments/assets/35000ad0-be05-44f7-aa1d-1eb0816ca572)

Ya podemos añadir todos los usuarios dentro de la base de datos, y en el MySQL se ve así.

![image](https://github.com/user-attachments/assets/da40331b-9c45-4d1a-bfed-a0598a076fb0)

Y así terminariamos la práctica de LAMP en 2 niveles.

## Autores
- **Kilian Gimenez** - [GitHub](https://github.com/Kilian-max) - Creador y jefe del proyecto.
