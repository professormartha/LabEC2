# LabEC2: Laboratorio para crear una instancia EC2
En este tutorial mostraré los pasos para crear una instancia EC2 en AWS
El objetivo es crear un pequeño sitio de Web 

## A. Preparación
### Security Group
A security group acts as a virtual firewall for your instance to control inbound and outbound traffic

1. Seleccionar el servicio EC2
2. Seleccionar del menú del lado izquierdo "Security Groups" en la sección Network & Security
3. Seleccionar el botón naranja <Create security group>
4. Sección “Basic details”
Security group name: allowsshttp
description: webserver
VPC: default
5. Sección “Inbound rules”
Seleccionar el botón <add a rule>
Adicionar los protocolos HTTP con 0.0.0.0/0 y SSH con tu IP
Selecciona el botón <create security group>

### Keypair
A key pair, consisting of a private key and a public key, is a set of security credentials that you use to prove your identity when connecting to an instance. Amazon EC2 stores the public key, and you store the private key. You use the private key, instead of a password, to securely access your instances

1. Seleccionar el servicio EC2
2. Seleccionar del menú del lado izquierdo “Key “pairs en la sección Network & Security
3. Seleccionar el botón naranja <Create key pair>
Name: alumno-keypair
Key pair type: RSA
file format: pem
Selecciona el botón naranja <create key pair>
Guarda en tu laptop el key pair alumno-keypair.pem

### Opcional. Cloud9
Crea un Cloud 9 
1. Sube el key pair alumno-keypair.pem

### B. Lanzamiento de una instancia EC2

1.  Seleccionar el servicio EC2
2. Seleccionar del menú del lado izquierdo la opción "Instances"
3. Seleccionar el botón naranja <Launch instances>
4. Sección “Name and tags”: Name: MiWebSever 
5. Sección “Amazon Machine Image” AMI: Seleccionar "Amazon Linux AWS” 2 AMI (HVM), SSD Volume Type" <Free tier eligible>
6. Sección: Instance Type  seleccionar t2.micro
7. Sección “Key pair (login)” selecciona las llaves que creaste: alumno-keypair.pem
8. Sección “Network”  Selecciona el recuadro: Select existing security group Selecciona el que creaste: allowsshhttp
9. Sección “configure Storage”, dejar todos los campos por default 
10. Sección “Advance Details”  en la parte de abajo escribir en el recuadro de User Data

#!/bin/bash
yum update -y
yum install httpd php php-mysql -y
systemctl start httpd.service
systemctl enable httpd.service
usermod -a -G apache ec2-user
chown -R ec2-user:apache /var/www
chmod 2775 /var/www && find /var/www -type d -exec sudo chmod 2775 {} \;
echo "<?php phpinfo(); ?>" > /var/www/html/phpinfo.php


11. Selecciona el botón <Launch Instance>

## C. Revisión de la instancia
1. En el tablero de EC2, selecciona Instances
2. Selecciona la instancia activa que creaste
3. Copia el DNS  o el IP público y ábrelo en un navegador


## D. Challenge

AA. Entra a Cloud9 y copia tus llaves alumno-keypair.pem
BB. Entra a tu instancia y modifica el index.html


Otro laboratorio que puedes realizar es https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-lamp-amazon-linux-2.html











