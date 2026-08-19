# FastAPI WebSockets — Chat en tiempo real

Proyecto práctico desarrollado como parte de mi capacitación en **FastAPI** (curso de Udemy), donde exploro la implementación de **WebSockets** para comunicación en tiempo real, aplicando además una refactorización hacia **Clean Architecture**.

## 📌 Sobre el proyecto

La aplicación simula un sistema de **salas de chat (rooms)** con autenticación por token, donde los usuarios pueden conectarse vía WebSocket a una sala específica y enviar/recibir mensajes (alerts) en tiempo real, los cuales quedan persistidos en base de datos.

Partiendo de una implementación inicial más simple (`api.py`, `models.py`, `rest_api.py`), el proyecto fue refactorizado siguiendo los principios de **Clean Architecture**, separando responsabilidades en capas independientes y desacopladas del framework.

## 🎯 Objetivos de aprendizaje

- Entender el protocolo WebSocket y su implementación en FastAPI (`WebSocket`, `WebSocketDisconnect`).
- Manejar múltiples conexiones activas simultáneas mediante un `ConnectionManager`.
- Aplicar autenticación por token tanto en endpoints REST como en conexiones WebSocket (vía query param).
- Persistir mensajes en base de datos con SQLAlchemy.
- Aplicar el patrón **Repository** para desacoplar el acceso a datos.
- Refactorizar una aplicación monolítica hacia **Clean Architecture** (Entities → Use Cases → Interface Adapters → Frameworks & Drivers).

## 🏗️ Arquitectura

El proyecto sigue las 4 capas de Clean Architecture, con las dependencias apuntando siempre hacia adentro:

```
src/
├── entities/                 # Modelos de negocio puros (sin dependencias de frameworks)
│   ├── user.py
│   ├── alert.py
│   ├── room.py
│   └── token.py
│
├── use_cases/                 # Lógica de aplicación (reglas de negocio)
│   ├── auth/
│   │   ├── login.py
│   │   ├── register.py
│   │   └── logout.py
│   ├── alerts/
│   │   ├── create_alert.py
│   │   └── get_alerts.py
│   └── rooms/
│       └── get_rooms.py
│
├── interface_adapters/        # Controladores, repositorios y esquemas
│   ├── controllers/
│   │   ├── auth_controller.py
│   │   ├── alerts_controller.py
│   │   ├── rooms_controller.py
│   │   └── websocket_controller.py
│   ├── repositories/
│   │   ├── repository_interfaces.py
│   │   ├── user_repository.py
│   │   ├── alert_repository.py
│   │   ├── room_repository.py
│   │   └── token_repository.py
│   └── presenters/
│       └── schemas.py
│
└── frameworks_drivers/        # Configuración de FastAPI y base de datos
    ├── db/
    │   ├── connection.py
    │   └── orm_models.py
    └── http/
        ├── app.py
        └── dependencies.py

main.py                        # Punto de entrada de la aplicación
```

> 📄 El detalle completo del proceso de refactorización está documentado en [`ESTRUCTURA_LIMPIA.md`](./ESTRUCTURA_LIMPIA.md).

## 🚀 Tecnologías utilizadas

- **FastAPI** — framework web para la API REST y WebSockets
- **Uvicorn** — servidor ASGI
- **WebSockets** — comunicación bidireccional en tiempo real
- **SQLAlchemy** — ORM para persistencia de datos
- **SQLite** — base de datos para desarrollo
- **Pydantic** — validación de datos y esquemas
- **bcrypt** — hasheo seguro de contraseñas
- **Jinja2** — motor de plantillas para la vista de chat de prueba

## 🔌 Endpoints principales

### REST API (`/api`)

| Método | Endpoint         | Descripción                             | Autenticación |
|--------|-------------------|------------------------------------------|----------------|
| POST   | `/api/login`      | Autentica al usuario y devuelve un token | No             |
| POST   | `/api/register`   | Crea una nueva cuenta de usuario         | No             |
| POST   | `/api/logout`     | Invalida el token del usuario            | No             |
| GET    | `/api/alerts`     | Lista mensajes (filtrable por `room_id`) | Sí             |
| GET    | `/api/rooms`      | Lista todas las salas disponibles        | No             |

### WebSocket

| Endpoint                          | Descripción                                                  |
|-----------------------------------|---------------------------------------------------------------|
| `WS /ws/alert/room/{room_id}?token=Token_<key>` | Conexión a una sala específica con autenticación por token |

## ⚙️ Cómo correr el proyecto

1. Clonar el repositorio:
```bash
git clone https://github.com/Ivan-Porcari/fastapi_websockets.git
cd fastapi_websockets
```

2. Crear y activar entorno virtual:
```bash
python -m venv .venv
source .venv/Scripts/activate   # Windows con Git Bash
```

3. Instalar dependencias:
```bash
pip install -r requirements.txt
```

4. Levantar el servidor:
```bash
uvicorn main:app --reload
```

5. Abrir en el navegador: `http://127.0.0.1:8000` para probar el chat de ejemplo, o `http://127.0.0.1:8000/docs` para explorar la documentación interactiva de la API.

## 📚 Contexto

Este proyecto forma parte de mi proceso de aprendizaje en el desarrollo backend con Python, como práctica derivada de un curso de **FastAPI en Udemy**, enfocado específicamente en el manejo de WebSockets para aplicaciones en tiempo real. Además de seguir el contenido del curso, tomé la iniciativa de refactorizar la solución aplicando principios de **Clean Architecture** para reforzar buenas prácticas de diseño de software.

---

**Autor:** Iván Porcari
