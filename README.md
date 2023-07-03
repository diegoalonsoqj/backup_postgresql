# backup_postgresql

El script es un programa de bash que realiza backups automáticos de bases de datos PostgreSQL. Guarda los respaldos en un directorio predefinido, registra eventos en un archivo de log y elimina respaldos antiguos. Utiliza las herramientas pg_dump y gzip para realizar el respaldo y compresión de las bases de datos. El script es configurable, permitiendo especificar el nombre de usuario, contraseña, host y puerto del servidor de PostgreSQL, así como el directorio de almacenamiento de los backups.

Aquí una descripción detallada y paso a paso del script de backup de bases de datos PostgreSQL:

El script inicia con la declaración del intérprete de comandos que utilizará, que es #!/bin/bash. Esto indica que el script será ejecutado utilizando el intérprete de comandos bash.

Luego, se definen varias variables para configurar el proceso de respaldo:

backup_dir: Especifica el directorio de almacenamiento donde se guardarán los respaldos.
db_user y db_password: Son las credenciales de acceso del usuario de PostgreSQL que realizará los respaldos.
db_host y db_port: Indican la dirección y el puerto del servidor PostgreSQL al que se conectará el script para realizar los respaldos.
current_datetime: Almacena la fecha y hora actual en un formato específico (año-mes-día_hora:minuto) para ser utilizado en los nombres de los archivos de respaldo.
A continuación, se crean los directorios de almacenamiento de logs ($log_dir) y el archivo de log ($log_file) para registrar eventos y mensajes durante la ejecución del script.

Se define la función log_message que se encarga de registrar mensajes en el archivo de log con fecha y hora. Esta función será llamada a lo largo del script para registrar eventos importantes.

Se registra el inicio del script en el archivo de log utilizando log_message para indicar que se ha iniciado el proceso de respaldo.

El script utiliza el comando psql para obtener la lista de bases de datos existentes en el servidor PostgreSQL. La lista se almacena en la variable database_list.

Se verifica si se encontraron bases de datos válidas en el servidor. Si no se encuentra ninguna base de datos, se registra un mensaje en el archivo de log y se sale del script con un código de salida 1.

Si se encuentran bases de datos válidas, el script convierte la lista de bases de datos en un array denominado databases.

El script itera a través de cada base de datos en el array databases y realiza el respaldo de cada una de ellas.

Para cada base de datos, el script crea el nombre del archivo de respaldo en el directorio de backups utilizando el nombre de la base de datos y la fecha y hora actual.

Se registra el inicio del respaldo de la base de datos en el archivo de log.

Utiliza el comando pg_dump para realizar el respaldo de la base de datos seleccionada. El respaldo se realiza en un formato personalizado (-F c) y se comprime con gzip, lo que genera un archivo .gz.

Se verifica si el respaldo se completó correctamente mediante el código de salida del comando pg_dump. Si es exitoso, se registra la finalización del respaldo y el tamaño del archivo de respaldo en el archivo de log.

Si el respaldo falla, se registra un mensaje de error en el archivo de log.

El script borra los archivos de respaldo con 2 o más días de antigüedad utilizando el comando find y registra esta acción en el archivo de log.

Finalmente, se registra el fin del script de respaldo en el archivo de log.