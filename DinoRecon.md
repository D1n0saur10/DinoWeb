# 🦕 ꧁𓊈Recon Pasivo𓊉꧂

**`⃤⃞⃟⃤`** Herramientas de reconocimiento pasivo.
```bash
https://bgp.he.net
https://stat.ripe.net
https://search.onyphe.io
https://www.nerdydata.com
https://hunter.how
https://otx.alienvault.com
https://www.zoomeye.ai
https://web.archive.org
https://urlscan.io
https://en.fofa.info
https://www.shodan.io
https://inteltechniques.com/tools/Search.html
dnsarchive.net
dnsdumpster.com
bigdomaindata.com
viewdns.info
https://www.virustotal.com/vtapi/v2/domain/report?apikey=YOUR_API_KEY&domain=example.com <<< subdominios
crt.sh
```
**`⃤⃞⃟⃤`** Google Dorks.
```bash
# Archivos
site:target (filetype:doc OR filetype:pdf OR filetype:xls OR filetype:txt OR filetype:ps OR filetype:rtf OR filetype:odt OR filetype:sxw OR filetype:psw OR filetype:ppt OR filetype:pps OR filetype:xml OR filetype:php OR filetype:sql)
--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
# Texto en documentos.
site:target.com (intext:"confidential salary" OR intext:"approved budget" OR intext:"payroll spreadsheet" OR intext:"executive compensation" OR intext:"financial forecast" OR intext:"fiscal budget" OR intext:"employee payroll" OR intext:"revenue projections" OR intext:"password" OR intext:"API key" OR intext:"access token" OR intext:"backup" OR intext:"internal config" OR intext:"confidential document" OR intext:"publicly exposed" OR intext:"misconfiguration" OR intext:"data leak" OR intext:"leaked credentials")
# Version en espanol.
site:target.com (intext:"salario confidencial" OR intext:"presupuesto aprobado" OR intext:"hoja de cálculo de nómina" OR intext:"compensación ejecutiva" OR intext:"pronóstico financiero" OR intext:"presupuesto fiscal" OR intext:"nómina de empleados" OR intext:"proyecciones de ingresos" OR intext:"contraseña" OR intext:"clave API" OR intext:"token de acceso" OR intext:"backup" OR intext:"configuración interna" OR intext:"archivo confidencial" OR intext:"documento interno" OR intext:"expuesto públicamente" OR intext:"error de configuración" OR intext:"filtración de datos")
-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
# Parametros comunes.
site:target.com inurl:"?" (inurl:"id=" OR inurl:"user=" OR inurl:"uid=" OR inurl:"pid=" OR inurl:"fid=" OR inurl:"page=" OR inurl:"pagina=" OR inurl:"view=" OR inurl:"vista=" OR inurl:"file=" OR inurl:"archivo=" OR inurl:"doc=" OR inurl:"documento=" OR inurl:"path=" OR inurl:"ruta=" OR inurl:"action=" OR inurl:"accion=" OR inurl:"cmd=" OR inurl:"comando=" OR inurl:"exec=" OR inurl:"ejecutar=" OR inurl:"token=" OR inurl:"sesion=" OR inurl:"session=" OR inurl:"login=" OR inurl:"admin=" OR inurl:"search=" OR inurl:"busqueda=" OR inurl:"q=" OR inurl:"redirect=" OR inurl:"redirigir=" OR inurl:"callback=" OR inurl:"retorno=" OR inurl:"api_key=" OR inurl:"clave_api=" OR inurl:"debug=" OR inurl:"depuracion=") (intext:"error" OR intext:"advertencia" OR intext:"sql" OR intext:"mysql" OR intext:"sintaxis" OR intext:"contraseña" OR intext:"password" OR intext:"usuario" OR intext:"username" OR intext:"secreto" OR intext:"secret" OR intext:"explotar" OR intext:"vulnerabilidad" OR intext:"inyección" OR intext:"injection" OR intext:"fallo" OR intext:"fallo de seguridad" OR intext:"warning" OR intext:"database") -inurl:logout -inurl:admin -inurl:cuenta -inurl:acceso -inurl:api -inurl:static -inurl:assets -inurl:img -inurl:css -inurl:js
---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
# Index.
site:target intitle:"index of"
----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
# Nombre exacto en diferentes paginas.
site:pastebin.com "target"
site:github.com "target"
site:controlc.com "target"
site:gitlab.com "target"
site:gist.github.com "target"
site:bitbucket.org "target"
site:gitea.com "target"
site:raw.githubusercontent.com "target"
site:sourceforge.net "target"
------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
# Errores comunes.
site:target.com intext:"error" OR intext:"warning" OR intext:"exception" OR intext:"traceback" OR intext:"syntax error" OR intext:"database error" OR intext:"django error" OR intext:"laravel error" OR intext:"Whoops! There was an error" OR intext:"Directory Listing of /" OR intext:".phpdebugbarr" OR intext:"APPPATH/" OR intext:"you have an error in your sql" OR intext:"PHP DebugBar | Open" OR intext:"Swagger-ui" OR intext:"php error" OR intext:"node error" OR intext:"template error" OR intext:"csrf token mismatch" OR intext:"500 internal server error" OR intext:"404 not found" OR intext:"debug mode" OR intext:"depuracion activada" OR intext:"sql syntax error" OR intext:"mysql error" OR intext:"access denied" OR intext:"permission denied" -inurl:logout -inurl:admin -inurl:cuenta -inurl:acceso -inurl:api -inurl:static -inurl:assets

```

