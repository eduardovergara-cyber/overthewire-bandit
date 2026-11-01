# 🧩 OverTheWire Bandit: notas de aprendizaje

Notas personales de lo que aprendí resolviendo [Bandit](https://overthewire.org/wargames/bandit/), el wargame de OverTheWire para practicar fundamentos de Linux y seguridad.

> ⚠️ Este repositorio **no contiene contraseñas ni soluciones paso a paso**. Documento los conceptos y comandos que aprendí, organizados por tema, para que sirvan de referencia y no arruinen el desafío a quien lo esté jugando.

**Progreso:** ✅ completado (niveles 0 → 33)

---

## 📚 Conceptos por tema

### 1. Conexión remota con SSH (niveles 0–1, 13–14)
- Conectarse a un servidor en un puerto no estándar: `ssh usuario@host -p PUERTO`
- Autenticación con **clave privada** en vez de contraseña: `ssh -i clave.key`
- Por qué los permisos de la clave importan: SSH rechaza claves legibles por otros (`chmod 600`)

**Relevancia en seguridad:** SSH es la puerta de entrada a casi cualquier servidor Linux; entender claves y permisos es la base del hardening de SSH.

### 2. Archivos con nombres problemáticos (niveles 1–3)
- Archivos llamados `-`, con espacios o ocultos (que empiezan con `.`)
- Uso de `./`, comillas y `ls -la` para verlos y leerlos

**Relevancia en seguridad:** los atacantes esconden archivos con nombres engañosos; saber encontrarlos es parte de revisar un sistema comprometido.

### 3. Búsqueda de archivos (niveles 4–6)
- Identificar el tipo real de un archivo con `file`, sin fiarse de la extensión
- `find` por tamaño, dueño, grupo y permisos: `find / -user X -group Y -size Nc 2>/dev/null`
- Descartar errores redirigiendo `stderr` a `/dev/null`

**Relevancia en seguridad:** `find` es la herramienta clave para auditar permisos y buscar archivos sospechosos.

### 4. Procesamiento de texto (niveles 7–9)
- `grep` para filtrar, `sort | uniq -u` para encontrar líneas únicas
- `strings` para extraer texto legible de archivos binarios
- Encadenar comandos con tuberías (`|`)

**Relevancia en seguridad:** es exactamente lo que se hace al analizar logs a mano.

### 5. Codificación y compresión (niveles 10–12)
- Base64 (`base64 -d`) y ROT13 con `tr`
- Volcados hexadecimales con `xxd -r`
- Identificar y descomprimir capas de gzip, bzip2 y tar con `file` en cada paso

**Relevancia en seguridad:** codificar **no es cifrar**; el malware y los atacantes ocultan datos así todo el tiempo.

### 6. Redes y puertos (niveles 14–18)
- Enviar datos a un puerto con `nc` (netcat)
- Conexiones cifradas con `openssl s_client`
- Escanear puertos abiertos con `nmap` y distinguir los que hablan SSL
- Comparar archivos con `diff`

**Relevancia en seguridad:** el reconocimiento de servicios es el primer paso de cualquier evaluación de seguridad.

### 7. Shells y permisos especiales (niveles 18–20)
- Ejecutar un comando por SSH cuando el `.bashrc` cierra la sesión
- Binarios **setuid**: se ejecutan con los permisos de su dueño
- Dejar `nc` escuchando en un puerto (`nc -lvp`) para recibir una conexión

**Relevancia en seguridad:** los binarios setuid mal configurados son una de las vías clásicas de escalada de privilegios.

### 8. Tareas programadas con cron (niveles 21–23)
- Leer la configuración de `/etc/cron.d/` y los scripts que ejecuta
- Entender qué usuario corre cada tarea y cuándo
- Aprovechar un script de cron que ejecuta archivos de un directorio con permisos de escritura

**Relevancia en seguridad:** revisar las tareas programadas es un paso obligado al auditar un servidor; un cron job inseguro es una vía real de escalada y de persistencia.

### 9. Automatización con Bash (nivel 24)
- Generar todas las combinaciones de un PIN con un bucle `for` y `seq`
- Enviarlas en una sola conexión a un servicio y filtrar la respuesta correcta

**Relevancia en seguridad:** muestra por qué los servicios necesitan límite de intentos; sin él, la fuerza bruta es trivial.

### 10. Escapar de shells restringidas (niveles 25–26, 32)
- Usuarios con una shell distinta de bash (revisada en `/etc/passwd`)
- Abrir una shell desde `more` y `vim` (`:set shell=/bin/bash` y `:shell`)
- Saltarse una shell que convierte todo a mayúsculas usando `$0`

**Relevancia en seguridad:** una shell restringida no es un control de seguridad suficiente si los programas permitidos pueden ejecutar comandos.

### 11. Git como fuente de información (niveles 27–31)
- Clonar repositorios por SSH en un puerto no estándar
- Buscar información sensible en el historial (`git log`, `git show`), en ramas (`git branch -a`) y en tags
- Cómo `.gitignore` y los hooks afectan lo que se puede subir

**Relevancia en seguridad:** borrar una contraseña en un commit nuevo no la elimina del historial; los secretos filtrados en Git son una causa frecuente de brechas.

---

## 🛠️ Comandos que más usé

`ssh` · `ls -la` · `cat` · `file` · `find` · `grep` · `sort` · `uniq` · `strings` · `base64` · `tr` · `xxd` · `tar` · `gzip` · `bzip2` · `nc` · `openssl` · `nmap` · `diff` · `crontab` · `vim` · `git`

## 💡 Qué me llevo

- Leer el manual (`man`) antes de buscar en internet
- Linux se maneja encadenando herramientas pequeñas
- Muchos problemas de seguridad vienen de permisos mal configurados
- Los puntos que revisaría al auditar un servidor: permisos, setuid, cron, servicios expuestos e historial de Git
