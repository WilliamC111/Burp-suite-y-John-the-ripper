# Burp-suite-y-John-the-ripper

# 🔐 Laboratorio de Seguridad con Kali Linux y Burp Suite

> Tutorial completo para configurar un entorno de pruebas de seguridad y realizar análisis de vulnerabilidades web

---

## 📋 Tabla de Contenidos

- [Descripción General](#-descripción-general)
- [Requisitos Previos](#-requisitos-previos)
- [Instalación del Entorno](#-instalación-del-entorno)
  - [VirtualBox](#1-virtualbox)
  - [Kali Linux](#2-kali-linux)
  - [Metasploitable2](#3-metasploitable2)
- [Configuración de Herramientas](#-configuración-de-herramientas)
  - [Burp Suite con Firefox](#1-burp-suite-con-firefox)
  - [Certificado CA de Burp](#2-certificado-ca-de-burp)
- [Prácticas de Seguridad](#-prácticas-de-seguridad)
  - [Fuerza Bruta con Intruder](#1-ataque-de-fuerza-bruta-con-intruder)
  - [Directory Traversal con Repeater](#2-directory-traversal-con-repeater)
  - [Inyección SQL](#3-inyección-sql)

---

## 🎯 Descripción General

Este laboratorio te permitirá:
- Configurar un entorno seguro y controlado para pruebas de penetración
- Practicar técnicas de análisis de vulnerabilidades web

---

## 💻 Requisitos Previos

### Hardware
- **RAM:** Mínimo 8 GB (recomendado 16 GB)
- **Almacenamiento:** 30 GB libres
- **Procesador:** Compatible con virtualización (VT-x/AMD-V)

### Software
- Sistema operativo Windows
- Conexión a Internet para descargas

---

## 🚀 Instalación del Entorno

### 1. VirtualBox
#### Descarga e Instalación
1. Visita la página oficial:
   ```
   https://www.virtualbox.org/
   ```
2. Descarga el instalador para Windows (archivo `.exe`)
3. Descarga Visual C++ Redistributable:
   ```
   https://learn.microsoft.com/es-es/cpp/windows/latest-supported-vc-redist?view=msvc-170
   ```
4. Ejecuta el instalador de VirtualBox como **Administrador**
5. Sigue el asistente con las configuraciones predeterminadas
6. Acepta la instalación de controladores firmados cuando se solicite

---

### 2. Kali Linux
#### Descarga de la ISO
1. Accede al sitio oficial:
   ```
   https://www.kali.org/get-kali/#kali-platforms
   ```
2. Descarga la imagen **Installer 64-bit** o **Live** según tu preferencia
3. Guarda el archivo `.iso` en tu equipo
#### Creación de la Máquina Virtual
1. Abre VirtualBox y haz clic en **Nueva**
2. Configura los siguientes parámetros:
   - **Nombre:** Kali Linux
   - **Tipo:** Linux
   - **Versión:** Debian 64-bit (o Other Linux 64-bit)
   - **ISO:** Selecciona la imagen descargada

3. Asignación de recursos:
   - **RAM:** 2048 MB mínimo (4096 MB recomendado)
   - **CPUs:** 2 cores
   - **Disco virtual:** 20-25 GB
4. Haz clic en **Finalizar**

#### Configuración de Red
1. Selecciona la VM de Kali y ve a **Configuración**
2. Ve a la sección **Network**
3. Cambia de **NAT** a **Bridged Adapter**
   > Esto permitirá que la VM esté en la misma red local que tu host

#### Configuración Adicional
1. En **General** → **Carpeta Compartida**
2. Habilita los campos como **bidireccionales**

#### Instalación del Sistema Operativo
1. Inicia la VM
2. Selecciona **Graphical Install** o **Install**
3. Sigue el asistente configurando:
   - Idioma
   - Distribución de teclado
   - Configuración de red
   - Hostname
   - Usuario y contraseña (puede ser usuario no-root con sudo)
4. Completa la instalación y reinicia

---
### 3. Metasploitable2
Esta VM vulnerable se utilizará como objetivo de pruebas.
#### Descarga
1. Visita:
   ```
   https://sourceforge.net/projects/metasploitable/files/Metasploitable2/
   ```
2. Descarga el archivo comprimido
3. Descomprime la carpeta en tu sistema

#### Configuración en VirtualBox
1. Abre VirtualBox y haz clic en **Nueva**
2. Configura:
   - **Nombre:** Metasploitable2
   - **Tipo:** Linux
   - **Versión:** Debian 64-bit o Other Linux
   - **Memoria:** 1024 MB mínimo
   - **CPUs:** 1
3. En **Disco duro virtual:**
   - Selecciona **Usar un archivo de disco duro virtual existente**
   - Navega y selecciona el archivo `.vdi` extraído
4. Aplica las configuraciones adicionales de red (Bridged Adapter)

---
## 🔧 Configuración de Herramientas
### 1. Burp Suite con Firefox
#### Configurar Burp Suite
1. Abre **Burp Suite** en Kali Linux
2. Ve a **Proxy** → **Options**
3. Verifica que Burp escuche en `127.0.0.1:8080`
   - Si no está configurado así, agrégalo manualmente

#### Configurar Firefox
1. Abre **Firefox**
2. Ve al menú → **Preferences**
3. Busca **Network Settings** o desplázate hasta encontrarlo
4. Haz clic en **Settings**
5. Selecciona **Manual proxy configuration**
6. Configura:
   - **HTTP Proxy:** `127.0.0.1`
   - **Port:** `8080`
   - Marca **Use this proxy for all protocols**
7. Haz clic en **OK**

#### Verificar Configuración
1. En Burp Suite, ve a **Proxy** → **Intercept**
2. Activa la intercepción (**Intercept is on**)
3. En Firefox, intenta cargar `google.com`
4. Verifica que la petición aparezca en **Proxy** → **HTTP history**
---

### 2. Certificado CA de Burp
Para evitar advertencias SSL en HTTPS:
1. Con Burp configurado, abre Firefox
2. Navega a: `http://burp`
3. Descarga el certificado CA haciendo clic en el enlace
4. Guarda el archivo `cacert.der`
5. En Firefox, ve a **Preferences**
6. Desplázate hasta **Privacy & Security**
7. En la sección **Certificates**, haz clic en **View Certificates**
8. Ve a la pestaña **Authorities**
9. Haz clic en **Import**
10. Navega a la ubicación de descarga (ej: `/home/tu_usuario/Downloads/cacert.der`)
11. Selecciona el archivo y haz clic en **Open**
12. Marca **Trust this CA to identify websites**
13. Haz clic en **OK**

---
## Prácticas de Seguridad
### 1. Ataque de Fuerza Bruta con Intruder
Esta práctica te enseñará a descubrir contraseñas débiles mediante fuerza bruta.

#### Paso 1: Descubrimiento del Objetivo
1. En Kali Linux, abre una terminal
2. Obtén tu IP local:
   ```bash
   ifconfig
   ```
3. Escanea la red para encontrar la IP de Metasploitable:
   ```bash
   nmap -sn 192.168.1.0/24
   ```
   > Ajusta el rango según tu red
#### Paso 2: Identificar Servicios
1. Escanea los puertos del objetivo:
   ```bash
   nmap -p- <IP_OBJETIVO>
   ```
2. Verifica que el puerto **80** (HTTP) esté abierto

#### Paso 3: Acceder a DVWA
1. En Firefox, navega a:
   ```
   http://<IP_OBJETIVO>
   ```
2. Accede a DVWA (Damn Vulnerable Web Application)
3. Inicia sesión con credenciales por defecto si es necesario
4. Navega a la sección **Brute Force**

#### Paso 4: Capturar la Petición
1. En DVWA, introduce credenciales incorrectas:
   - Usuario: `admin`
   - Contraseña: `admin`
2. Presiona **Login**
3. En Burp Suite, ve a **Proxy** → **HTTP history**
4. Localiza la petición de login

#### Paso 5: Configurar Intruder
1. Haz clic derecho en la petición → **Send to Intruder** (o `Ctrl+I`)
2. Ve a la pestaña **Positions**
3. Haz clic en **Clear** para eliminar variables automáticas
4. Selecciona solo el valor de la contraseña (ej: `password=admin`)
5. Haz clic en **Add** para marcarlo como posición de ataque

#### Paso 6: Configurar Payloads
1. Ve a la pestaña **Payloads**
2. En **Payload type**, selecciona **Simple List**
3. Haz clic en **Load** y carga un archivo de diccionario de contraseñas
   > Puedes usar `claves.txt` en Kali
4. O agrega contraseñas manualmente con **Add**
   
#### Paso 7: Ejecutar el Ataque
1. Haz clic en **Start Attack**
2. Observa las respuestas en la nueva ventana
3. Analiza las columnas:
   - **Length:** Longitud de la respuesta
   - **Status:** Código de estado HTTP
4. Identifica la contraseña correcta:
   - Respuestas incorrectas tendrán longitud similar
   - La respuesta correcta tendrá longitud diferente (mostrará "Welcome")
---

### 2. Directory Traversal con Repeater
Esta práctica demuestra cómo acceder a archivos del sistema mediante vulnerabilidades de inclusión de archivos.
#### Paso 1: Preparación
1. En DVWA, navega a la sección **File Inclusion**
2. Observa cómo la URL incluye un parámetro `page`

#### Paso 2: Capturar Petición
1. Selecciona cualquier opción en File Inclusion
2. En Burp Suite, localiza la petición en **HTTP history**
3. Envía la petición a **Repeater** (`Ctrl+R`)

#### Paso 3: Explotar la Vulnerabilidad
1. En Repeater, localiza el parámetro `page` en la URL
2. Modifica su valor utilizando Directory Traversal:
   ```
   page=../../../../etc/passwd
   ```
3. Haz clic en **Send**
4. Observa la respuesta en el panel derecho
5. Si es exitoso, verás el contenido del archivo `/etc/passwd` que contiene usuarios del sistema

#### Explicación
- `../` permite retroceder un directorio
- Al encadenar múltiples `../`, navegamos hasta la raíz del sistema
- Desde ahí, accedemos a `/etc/passwd`
---

### 3. Inyección SQL
Esta práctica muestra cómo explotar vulnerabilidades de inyección SQL.
#### Paso 1: Identificar el Punto de Entrada
1. En DVWA, navega a **SQL Injection**
2. Observa que hay un campo de entrada para ID de usuario

#### Paso 2: Generar Petición Base
1. Introduce un ID válido (ej: `1`)
2. Envía el formulario
3. 
#### Paso 3: Capturar en Burp
1. Ve a **Proxy** → **HTTP history**
2. Localiza la petición GET o POST
3. Envía a **Repeater** (`Ctrl+R`)

#### Paso 4: Probar Inyección Básica
1. En Repeater, modifica el parámetro `id`:
   ```
   id=1'
   ```
2. Haz clic en **Send**
3. Observa si aparece un error de SQL en la respuesta

#### Paso 5: Inyección con Operador Lógico
1. Modifica el payload a:
   ```
   id=1' OR 1=1--
   ```
2. Envía la petición
3. Analiza la respuesta:
   - Si la inyección funciona, verás **todos** los usuarios de la base de datos
   - `OR 1=1` hace que la condición siempre sea verdadera
   - `--` comenta el resto de la consulta SQL

#### Payloads Adicionales para Probar
```sql
' OR '1'='1
' OR '1'='1'--
' OR '1'='1' ({
' OR '1'='1' /*
admin'--
admin' #
admin'/*
```

#### Análisis de Resultados
- **Error de SQL:** Vulnerabilidad confirmada
- **Más datos devueltos:** La inyección fue exitosa
- **Sin cambios:** El campo puede estar protegido

---
## Recursos Adicionales
- [Documentación oficial de Burp Suite](https://portswigger.net/burp/documentation)
- [Kali Linux Documentation](https://www.kali.org/docs/)
- [OWASP Testing Guide](https://owasp.org/www-project-web-security-testing-guide/)
- [DVWA GitHub](https://github.com/digininja/DVWA)

---
