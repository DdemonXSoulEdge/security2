
auth_service: Registro, login y validación de tokens.

task_service: CRUD de tareas (restringido por token).

api_gateway: Punto de entrada único para clientes.

Base de datos: SQLite.

Estructura del Proyecto
Copiar código
micro/
├── api_gateway/
│   └── app.py
├── auth_service/
│   └── app.py
├── task_services/
│   └── app.py
├── venv/
├── start_services.sh
└── README.md
Python 3.8+

pip

Entorno virtual: python -m venv venv

Instalar dependencias:

bash
Copiar código
source venv/bin/activate
pip install flask requests pyjwt
Ejecutar Servicios
Puedes usar el script:

bash
Copiar código
chmod +x start_services.sh
./start_services.sh
Este script levanta:

API Gateway en http://localhost:5000

Auth Service en http://localhost:5001

Task Service en http://localhost:5003

Endpoints
Autenticación (a través del Gateway)
Base: http://localhost:5000/auth

Método	Ruta	Descripción
POST	/register	Registro de usuario
POST	/login	Retorna token de acceso

Ejemplo (ThunderClient / Postman):
POST http://localhost:5000/auth/register
Body:

json
Copiar código
{
  "username": "kuma",
  "password": "bonney"
}
POST http://localhost:5000/auth/login
Body:

json
Copiar código
{
  "username": "kuma",
  "password": "bonney"
}
Respuesta:

json
Copiar código
{
  "message": "Logged in successfully",
  "token": "eyJhbGciOi..."
}

Tareas (protegido por token)
Base: http://localhost:5003

Recuerda incluir en los headers:

makefile
Copiar código
Authorization: <TOKEN>
Endpoints:
Método	Ruta	Descripción
GET	/tasks	Obtener todas las tareas
GET	/tasks/<id>	Obtener tarea por ID
POST	/tasks	Crear una nueva tarea
PUT	/tasks/<id>	Actualizar una tarea
DELETE	/tasks/<id>	Eliminar una tarea

Ejemplo POST:
URL: http://localhost:5003/tasks
Headers:

pgsql
Copiar código
Authorization: <token>
Content-Type: application/json
Body:

json
Copiar código
{
  "name_task": "Nueva tarea",
  "desc_task": "Descripción de prueba",
  "created_of": "2025-06-27",
  "deadline": "2025-07-01",
  "status": 1,
  "isActive": true
}
Token JWT
Los tokens generados por auth_service duran 10 minutos (600 segundos). El task_service valida cada solicitud consultando el token.

