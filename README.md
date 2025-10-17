🔐 PASSWORD MANAGER CON DNI ELECTRÓNICO (DNIe)
=====================================================

📘 DESCRIPCIÓN DEL PROYECTO
-----------------------------------------------------
Este proyecto implementa un GESTOR DE CONTRASEÑAS SEGURO en Python que utiliza el DNI ELECTRÓNICO (DNIe) para proteger el acceso a la base de datos.

La aplicación cifra todas las contraseñas con una CLAVE SIMÉTRICA (AES/Fernet), la cual está ENVUELTA (cifrada) mediante la CLAVE PÚBLICA del chip del DNIe.  
Para acceder a la base de datos, el usuario debe introducir el PIN del DNIe, y el chip se encarga de DESCIFRAR la clave simétrica.

El objetivo del proyecto es combinar CRIPTOGRAFÍA SIMÉTRICA / ASIMÉTRICA, gestión segura de credenciales y autenticación mediante hardware seguro (DNIe).


🧩 FUNCIONALIDADES PRINCIPALES
-----------------------------------------------------
✅ Generación y cifrado de base de datos (passwords.db)
✅ Envoltura y desenvoltura de clave con el DNIe (RSA-OAEP)
✅ Desbloqueo seguro con PIN del DNIe
✅ Añadir, listar, editar y recuperar contraseñas
✅ Exportación individual de entradas a JSON
✅ Bloqueo de clave en memoria
✅ Compatible con Windows y Linux


🗂️ ESTRUCTURA DEL PROYECTO
-----------------------------------------------------
PasswordManager_DNIe/
│
├── Main.py                → Programa principal (CLI)
├── Storage_functions.py   → Funciones de cifrado y almacenamiento
├── state.py               → Control del estado (bloqueado/desbloqueado)
│
├── metadata/
│   └── metadata.json      → Contiene la clave envuelta
│
└── passwords/
    ├── passwords.db       → Base de datos cifrada
    └── entries_files/     → Archivos JSON de cada entrada


⚙️ INSTALACIÓN Y CONFIGURACIÓN
-----------------------------------------------------

1️⃣ REQUISITOS
-----------------------------------------------------
- Python 3.8 o superior
- OpenSC instalado (drivers del DNIe)
- Lector de DNIe funcional
- DNI electrónico con PIN activo


2️⃣ INSTALAR DEPENDENCIAS PYTHON
-----------------------------------------------------
Desde CMD o PowerShell, dentro de la carpeta del proyecto:

pip install cryptography PyKCS11


🧠 FUNCIONAMIENTO GENERAL
-----------------------------------------------------
El programa se ejecuta desde línea de comandos y cifra todas las contraseñas.  
Solo el propietario del DNIe y su PIN pueden acceder.


💻 COMANDOS PRINCIPALES
-----------------------------------------------------

🔹 INICIALIZAR BASE DE DATOS
python Main.py init

Crea una base de datos cifrada vacía y un archivo metadata.json con la clave en formato RAW_BASE64.

Salida esperada:
DB inicializada y cifrada localmente.
ATENCIÓN: actualmente la clave está guardada como RAW_BASE64.
Ejecuta 'wrap' para activar el modo seguro con DNIe.


🔹 ENVOLVER CLAVE CON DNIe (MODO SEGURO)
python Main.py wrap

Cifra la clave de la base de datos con la clave pública del DNIe (RSA-OAEP).

Salida esperada:
k_db envuelta correctamente con la clave pública del DNIe.
metadata.json actualizado (wrap_alg = RSA-OAEP)


🔹 DESBLOQUEAR BASE DE DATOS
python Main.py unlock

Pide el PIN del DNIe y usa la clave privada del chip para descifrar la base de datos.

Salida esperada:
Ejecutando unlock (modo seguro con DNIe)...
Introduce PIN del DNIe:
[PIN del usuario]
DB desbloqueada correctamente.


🔹 BLOQUEAR BASE DE DATOS
python Main.py lock

Elimina la clave de memoria (seguridad adicional).

Salida esperada:
DB bloqueada en memoria. Para volver a usarla, inserta DNIe y usa 'unlock' con PIN.


🔹 AÑADIR NUEVA CONTRASEÑA
python Main.py add "Gmail" -u "usuario@gmail.com" -p "miClaveSegura123" -n "Cuenta personal"

Salida esperada:
Entry 'Gmail' añadida y DB ordenada.
Archivo guardado en passwords/entries_files/1_Gmail.json


🔹 LISTAR CONTRASEÑAS GUARDADAS
python Main.py list

Salida esperada:
Listado de entradas:
1: Gmail (usuario@gmail.com
)
2: Facebook (user123)
Archivos individuales guardados en passwords/entries_files


🔹 OBTENER UNA CONTRASEÑA CONCRETA
python Main.py get 1

Salida esperada:

  id: 1,
  name: "Gmail",
  username: "usuario@gmail.com",
  password: "miClaveSegura123",
  notes: "Cuenta personal",
  created: "2025-10-17T12:34:56.789Z"

Archivo guardado en passwords/entries_files/1_Gmail_get.json


🔹 EDITAR UNA ENTRADA EXISTENTE
python Main.py edit 1

Salida esperada:
Editando entrada ID 1 (dejar vacío para no cambiar):
Nombre [Gmail]: Gmail Personal
Username [usuario@gmail.com]:
Password [miClaveSegura123]: nuevaClaveSegura!
Notas [Cuenta personal]: Cuenta principal
Entrada ID 1 actualizada correctamente.

🧪 FLUJO DE PRUEBA COMPLETO
-----------------------------------------------------
python Main.py init
python Main.py wrap
python Main.py unlock
python Main.py add "Gmail" -u "usuario@gmail.com" -p "claveSegura"
python Main.py list
python Main.py get 1
python Main.py edit 1
python Main.py lock


🧠 SEGURIDAD IMPLEMENTADA
-----------------------------------------------------
- Cifrado simétrico (AES/Fernet)
- Envoltura asimétrica (RSA-OAEP) mediante chip del DNIe
- PIN personal para desencriptar
- Clave solo en memoria (no en disco)
- Separación de metadatos y base de datos cifrada


⚠️ LIMITATIONS / KNOWN ISSUES
-----------------------------------------------------
- Solo se soporta un lector/DNIe a la vez.
- No hay manejo avanzado de errores de corrupción de DB.


💡 RECOMENDACIÓN
-----------------------------------------------------
• Usa ramas (branches) para implementar nuevas funcionalidades y para rastrear bugs o issues.
• Mantén el código principal estable en la rama `Main`.


👩‍💻 AUTORES Y CRÉDITOS
-----------------------------------------------------
Proyecto desarrollado para la asignatura SEGURIDAD EN REDES Y SERVIVIOS (2025).
Desarrollado por: Edgar González y Jorge Martín

Lenguaje: Python 3.10  
Dependencias:
- cryptography
- PyKCS11
- OpenSC (drivers PKCS#11 para DNIe)


📄 LICENCIA
-----------------------------------------------------
Uso académico y educativo.  
Código abierto para fines formativos y demostrativos.

