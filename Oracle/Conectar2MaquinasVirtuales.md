# Pasos para conectar dos maquinas virtuales e instanciar Oracle
Esta guía detalla el proceso para conectar una base de datos Oracle, instalada en una máquina virtual de CentOS, a un cliente SQL Developer que se encuentra en una máquina virtual separada de Windows 7.

Previo a iniciar se debe tener instalado el ``` SQL Developer ``` en la maquina con Windows 7
## 1. Configuración de red:
Usamos el adaptador de red en modo puente con la conexión Wi-Fi para que las máquinas virtuales se comuniquen con la red principal, las dos máquinas deben quedar iguales.

<img src="../Images/Conexion.png" alt="Diagrama de conexión" width="500">

## 2. Verificación de la red:
Confirmamos que las máquinas se puedan "ver" mutuamente mediante un ping exitoso.

Buscamos la ip del servidor donde se encuentra oracle instalado:

<img src="../Images/ifconfig.png" alt="Diagrama de conexión" width="500">

Y le hacemos un ping desde la maquina donde se va a usar el servicio:

<img src="../Images/ping.png" alt="Diagrama de conexión" width="500">

## 3. Configuración de firewall:
Abrimos el puerto 1521 en el firewall de la maquina oracle para permitir la conexión de la base de datos.
Se usan los siguientes comandos en la terminal:

``` 
sudo firewall-cmd --permanent --add-port=1521/tcp
```
```     
sudo firewall-cmd --reload
```

## 4. Configuración del entorno
Establecer las variables de entorno ORACLE_HOME y PATH de forma permanente en el archivo .bashrc para poder ejecutar los comandos de Oracle sin problemas en la terminal.

Sepuede abrir con el editor de texto ``` vi ``` o ``` nano ``` desde la terminal

``` 
vi ~/.bashrc
``` 

Agregamos al final del archivo las variables que necesitamos
``` 
# Oracle Environment Variables
export ORACLE_HOME=/opt/oracle/product/21c/dbhomeXE
export PATH=$ORACLE_HOME/bin:$PATH
``` 
Presiona ``` Esc ```, luego ``` :wq ``` y ``` Enter ``` para guardar los cambios

Para que los cambios surtan efecto en tu terminal actual, ejecuta el siguiente comando:
``` 
source ~/.bashrc
``` 

## 5. Verificación del servicio:
Confirmamos que el listener de Oracle esté activo y que la base de datos esté registrada.
``` 
lsnrctl status
``` 

Si sale ``` TNS:no listener ``` es porque se debe iniciar con el siguiente comando:
``` 
lsnrctl start
``` 
Ahora que el listener está corriendo, necesitamos que la base de datos xepdb1 se registre con él para que las conexiones desde SQL Developer funcionen.
``` 
sqlplus sys/contraseñasys as sysdba
``` 
Una vez que te conectes y veas el prompt ``` SQL> ```,  ejecuta el siguiente comando:
``` 
ALTER SYSTEM REGISTER;
``` 
Deberías ver un mensaje que dice ``` System altered ``` . (Sistema alterado).

Escribe ``` exit ```  para salir de SQL*Plus.

## 6. Conectar el sql developer
En la máquina que no tiene el Oracle configurar la conexión de la siguiente manera:

<img src="../Images/ConfiguracionSQL.png" alt="Diagrama de conexión" width="500">

Y LISTO ESO ERA TODO :)