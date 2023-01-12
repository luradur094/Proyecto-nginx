## Instalacion y caso practico de Nginx
Ahora pasaremos a su instalacion y a un pequeño caso practico donde configuraremos una pagina propia. Lo primero antes de instalar sera ver la version en la que esta el Ngnix poniendo el comando (apt policy nginx)

![..](https://github.com/luradur094/Proyecto-nginx/blob/main/Imagenes/Version.png)

Ya visto la version procedemos a actualizar todos nuestros paquetes del sistema con el comando (apt update && apt upgrade)

![..](https://github.com/luradur094/Proyecto-nginx/blob/main/Imagenes/Actualizar%20paquetes.png)

Ya listo todo actualizado pasamos a su instalacion con el siguiente comando (apt install nginx)

![..](https://github.com/luradur094/Proyecto-nginx/blob/main/Imagenes/Instalar%20nginx.png)

### Comprobacion y configuracion del firwall
Ya esta instalado el programa ahora pasaremos a configurarlo para ello pondremos el siguiente comando (ufw app list) este comando nos dara un listado como este 

!IMPORTANTE¡ Antes de usar el comando tienes que comprobar que el comando ufw este instalado con los comando (apt policy ufw) y para instalar (apt install ufw) 

![..](https://github.com/luradur094/Proyecto-nginx/blob/main/Imagenes/lista.png)

Nos mostrara tres resultados como estos, los perfiles de nginx que nos interesan son esos:
Nginx Full: Habre los puertos 80 y 443
Nginx HTTP: Habre solo el puerto 80
Nginx HTTPS: Habre solo el puerto 443

En estos casos es recomandable habrir el mas restrictivo en este caso permitiremos el trafico del puerto 80 con este comando (ufw allow 'Nginx HTTP') y lo comprobaremos con este comando (ufw status)

!IMPORTANTE¡ Antes ahi que activar el firewall que viene desactivado para ello se usa el comando (ufw enable)

![..](https://github.com/luradur094/Proyecto-nginx/blob/main/Imagenes/activar%20puerto%2080.png)

![..](https://github.com/luradur094/Proyecto-nginx/blob/main/Imagenes/comprobar%20status%20del%20firewall.png)

### Comprobacion del estado de nuestro Nginx y si funciona la pagina 
Ya esta todo configurado ahora comprobaremos si nuestro srvicio esta corriendo para ello ponemos el siguiente comando (systemctl status nginx)

![..](https://github.com/luradur094/Proyecto-nginx/blob/main/Imagenes/nginx%20corriendo.png)

Ahora comprobamos que funciona en algun navegador poniendo http://(nuestra IP) para saberla ponemos el comando (ip a) y comprobamos que salga esta pagina

![..](https://github.com/luradur094/Proyecto-nginx/blob/main/Imagenes/Comprobar%20funciona%20nginx.png)

### Configurar la pagina por defecto
Para ello nos metemos en el fichero /var/www/html y configuramos el fichero index.nginx-debian.html en mi caso pondre esto en el fichero ese yo e clonado el fichero original con el comando (cp index.nginx-debian.html index.html) y puse lo siguiente

![..](https://github.com/luradur094/Proyecto-nginx/blob/main/Imagenes/Configurar%20index.png)

Y una vez echo lo comprobamos poniendo en el navegador http://(nuestra IP) 

![..](https://github.com/luradur094/Proyecto-nginx/blob/main/Imagenes/comprobacion%20de%20que%20funciona.png) 

### Balance de carga
El balance de carga es un concepto que usa la administracion de sistemas informaticos refiriendose a la tecnica usada para compartir el trabajo a realizar entre varios ordenadores, procesos, discos u otros recursos

Esta practica se debe realizar con ters maquinas con nginx instalado en cada una. Una tendra la pagina web que usare la maquina configurada anteriormente otra tendra una segunda pagina configurada y la ultima actuara de balance de carga, estando las tres en la misma red.

- Segunda maquina

![..](https://github.com/luradur094/Proyecto-nginx/blob/main/Imagenes/maquina%20segunda.png)

- Maquina de balance de carga

![..](https://github.com/luradur094/Proyecto-nginx/blob/main/Imagenes/maquina%20de%20balance.png)

### Configuracion de la segunda maquina
1. Instalamos el nginx 
2. Configuramos la pagina web que viene por defecto:

![..](https://github.com/luradur094/Proyecto-nginx/blob/main/Imagenes/pagina%20de%20la%20segunda.png)

3. Comprobamos que funciona

![..](https://github.com/luradur094/Proyecto-nginx/blob/main/Imagenes/Prueba%20pagina%20segunda.png)

### Configuracion de la maquina que sera balance de carga
1. Borrar archivo de configuracion predeterminado del sitio virtual y crear un fichero en la ruta de la imagen

![..](https://github.com/luradur094/Proyecto-nginx/blob/main/Imagenes/fichero%20para%20configurar%20nueva%20pagina%20balance.png)

2. Añadir lineas en el fichero creado
- Upstream backend: Noas permiteel añadir los servidores de las paginas web por la ip de la red por la que se estan comunicando. Aparte debemos establecer el modo de balance que se usara en mi caso usare el "IP_HASH" la cual permite la persistencia de la sesion.

- Server: Indica la ip del servidor de balanceo y la configuracion 

![..](https://github.com/luradur094/Proyecto-nginx/blob/main/Imagenes/fichero%20config%20balance.png)

3. Comprobacion de su funcionamiento 
- Ya todo listo comprobamos que todo esta escrito bien con el comando (nginx -t) saliendo este mensaje 

![..](https://github.com/luradur094/Proyecto-nginx/blob/main/Imagenes/reinicio%20y%20errores%20de%20sintaxis.png)

- Reiniciar el servicio nginx
- Y ya todo listo en el buscador ponemos la ip de la maquina de balance de carga y nos saldra la pagina web principal y despues de unos segundos y refrescar la pagina saldra la segunda pagina

![..](https://github.com/luradur094/Proyecto-nginx/blob/main/Imagenes/prueba%20pagina%20balance.png)
