# RadioDIY

Alquila un vps 

# Guía de Instalación y Configuración de Icecast2

## 📌 Requisitos
- Servidor Linux (Ubuntu/Debian/CentOS)
- Acceso root o sudo
- Mínimo 512MB RAM (recomendado 1GB+)
- Conexión estable con buen ancho de banda

## 🔧 Instalación

### Ubuntu/Debian


~~~
sudo apt update
sudo apt install icecast2
~~~

## ⚙️ Configuración Básica

Archivo principal: /etc/icecast2/icecast.xml


~~~
nano /etc/icecast2/icecast.xml
~~~

~~~

<icecast>
    <!-- Ubicación y admin son cadenas visibles en la interfaz web -->
    <location>Earth</location>
    <admin>icemaster@localhost</admin>

    <!-- ¡IMPORTANTE! 
         Para usuarios nuevos:
         Comience solo cambiando las contraseñas y reinicie Icecast.
         Documentación completa en: http://icecast.org/docs/
    -->

    <limits>
        <clients>100</clients>
        <sources>2</sources>
        <queue-size>524288</queue-size>
        <client-timeout>30</client-timeout>
        <header-timeout>15</header-timeout>
        <source-timeout>10</source-timeout>
        <!-- Mejora el tiempo de conexión inicial pero aumenta la latencia -->
        <burst-on-connect>1</burst-on-connect>
        <burst-size>65535</burst-size>
    </limits>

    <authentication>
        <!-- Credenciales para fuentes de audio -->
        <source-password>XXXXXXXXXXXX</source-password>
        <!-- Credenciales para relays -->
        <relay-password>XXXXXXXXXXXXXX</relay-password>
        <!-- Credenciales para administración -->
        <admin-user>admin</admin-user>
        <admin-password>XXXXXXXXXXX</admin-password>
    </authentication>

    <!-- Hostname público del servidor -->
    <hostname>42.XX.XX.57</hostname>

    <!-- Configuración de puertos -->
    <listen-socket>
        <port>8000</port>
    </listen-socket>

    <!-- Configuración SSL (comentada por defecto) -->
    <!--
    <listen-socket>
        <port>8443</port>
        <ssl>1</ssl>
    </listen-socket>
    -->

    <!-- Cabeceras HTTP globales -->
    <http-headers>
        <header name="Access-Control-Allow-Origin" value="*" />
    </http-headers>

    <!-- Configuración de relay (comentada por defecto) -->
    <!--
    <master-server>127.0.0.1</master-server>
    <master-server-port>8000</master-server-port>
    <master-update-interval>120</master-update-interval>
    <master-password>hackme</master-password>
    -->

    <!-- Punto de montaje principal -->
    <mount>
        <mount-name>/radiovolketainterestelar.ogg</mount-name>
        <username>source</username>
        <password>XXXXXXXXXXXXXX</password>
        <max-listeners>1000</max-listeners>
        <burst-size>65536</burst-size>
        <stream-name>Radio Volketa Interestelar</stream-name>
        <genre>Bareta</genre>
        <public>1</public>

 <allow-metadata>1</allow-metadata>

    </mount>


    <!-- Habilitar servicio de archivos -->
    <fileserve>1</fileserve>

    <!-- Configuración de rutas -->
    <paths>
        <basedir>/usr/share/icecast2</basedir>
        <logdir>/var/log/icecast2</logdir>
        <webroot>/usr/share/icecast2/web</webroot>
        <adminroot>/usr/share/icecast2/admin</adminroot>
        <!-- Redirección de la raíz a la página de estado -->
        <alias source="/" destination="/status.xsl"/>
    </paths>

    <!-- Configuración de logs -->
    <logging>
        <accesslog>access.log</accesslog>
        <errorlog>error.log</errorlog>
        <loglevel>3</loglevel> <!-- 3 = Info -->
        <logsize>10000</logsize> <!-- Tamaño máximo en KB -->
    </logging>

    <!-- Configuración de seguridad -->
    <security>
        <chroot>0</chroot>
        <!--
        <changeowner>
            <user>nobody</user>
            <group>nogroup</group>
        </changeowner>
        -->
    </security>
</icecast>
~~~



SETUP VPS 


~~~

sudo ufw allow 8000/tcp
sudo ufw enablesudo ufw allow 8000/tcp
sudo ufw enable
~~~
