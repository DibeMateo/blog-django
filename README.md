# Blog Web con Django

Blog web desarrollado con Django como proyecto final del curso de Python de Coderhouse. Permite publicar artículos con imágenes, registrarse, iniciar sesión y tener un perfil de autor con biografía y avatar.

## Tecnologías

- Python 3
- Django
- SQLite (base de datos por defecto)
- Pillow (manejo de imágenes)
- python-decouple (variables de entorno)
- HTML y CSS (templates de Django)

## Funcionalidades principales

- Listado de publicaciones y detalle de cada post (visibles para cualquier visitante).
- Crear, editar y eliminar posts desde la interfaz web (solo usuarios autenticados).
- Imagen opcional en cada post. Si un post no tiene imagen, la página se ve igual.
- Registro de usuarios, inicio y cierre de sesión.
- Perfil de usuario con biografía, link web y avatar, editable.
- Menú que cambia según el usuario esté autenticado o sea anónimo.
- Panel de administración de Django.

## Instalación

Requisitos: Python 3.10 o superior y Git.

### 1. Clonar el repositorio

```bash
git clone https://github.com/DibeMateo/blog-django.git
cd blog-django
```

### 2. Crear y activar el entorno virtual

macOS / Linux:

```bash
python3 -m venv venv
source venv/bin/activate
```

Windows (PowerShell):

```powershell
python -m venv venv
venv\Scripts\activate
```

### 3. Instalar las dependencias

```bash
pip install -r requirements.txt
```

### 4. Variables de entorno (opcional)

El proyecto corre sin configurar nada. Si querés usar tu propia clave:

```bash
cp .env.example .env
```

y completá `SECRET_KEY` en el archivo `.env` (que no se sube a GitHub).

### 5. Crear la base de datos

El repositorio no incluye `db.sqlite3`; se genera con las migraciones:

```bash
python manage.py migrate
```

### 6. Iniciar el servidor

```bash
python manage.py runserver
```

Abrir en el navegador: http://127.0.0.1:8000/

### 7. Crear un superusuario (opcional, para el panel de administración)

```bash
python manage.py createsuperuser
```

Completá usuario, email y contraseña cuando lo pida. El admin está en http://127.0.0.1:8000/admin/

## Cómo probar el proyecto desde cero

El repositorio no incluye base de datos ni imágenes subidas. Para probarlo:

1. Registrarse en `/usuarios/registro/` (queda logueado automáticamente).
2. Crear un post en `/posts/nuevo/`, con o sin imagen. **Tiene que tener estado "Publicado" para aparecer en el listado.**
3. Editar el perfil en `/usuarios/perfil/editar/` y subir un avatar.
4. Cerrar sesión y verificar que el listado sigue visible pero crear, editar y eliminar redirigen al login.

## Rutas

| Ruta | Descripción | Requiere login |
|---|---|---|
| `/` | Inicio | No |
| `/acerca/` | Acerca de | No |
| `/posts/` | Listado de publicaciones | No |
| `/posts/<id>/` | Detalle de un post | No |
| `/posts/nuevo/` | Crear post | **Sí** |
| `/posts/<id>/editar/` | Editar post | **Sí** |
| `/posts/<id>/eliminar/` | Eliminar post (confirmación y borrado por POST) | **Sí** |
| `/usuarios/registro/` | Registro | No |
| `/usuarios/login/` | Iniciar sesión | No |
| `/usuarios/logout/` | Cerrar sesión (solo por POST) | **Sí** |
| `/usuarios/perfil/` | Ver perfil | **Sí** |
| `/usuarios/perfil/editar/` | Editar biografía, link web y avatar | **Sí** |
| `/admin/` | Panel de administración | Superusuario |

La protección está en las vistas con `@login_required`. Si un visitante anónimo entra directo a una ruta protegida, Django lo redirige al login (`LOGIN_URL`) y, al autenticarse, vuelve a la página pedida.

## Usuarios y perfil

- **Registro:** `RegistroForm` extiende `UserCreationForm` y agrega el email. Al guardar el usuario se crea su `Perfil` y se inicia sesión.
- **Login / logout:** vistas `LoginView` y `LogoutView` de `django.contrib.auth`. El cierre de sesión se hace con un formulario POST con `{% csrf_token %}`.
- **Modelo `Perfil`:** relación `OneToOneField` con `User`, con `biografia`, `link_web` y `avatar`.

## Imágenes

- Las imágenes de posts se guardan en `media/posts/` y los avatares en `media/avatares/`.
- `MEDIA_URL = '/media/'` y `MEDIA_ROOT = BASE_DIR / 'media'` en `settings.py`. En desarrollo (`DEBUG = True`) Django las sirve con `static()` en el `urls.py` principal.
- Los formularios con archivos usan `enctype="multipart/form-data"` y las vistas pasan `request.FILES`.
- La carpeta `media/` no se sube al repositorio: se crea sola al subir la primera imagen.

## Estructura del proyecto

```
blog-django/
├── manage.py
├── requirements.txt
├── .env.example
├── .gitignore
├── README.md
├── blog_project/      # configuración (settings.py, urls.py)
├── posts/             # app de publicaciones (modelos, vistas, templates, estáticos)
└── usuarios/          # app de usuarios (registro, perfil, templates)
```

## Autor

Mateo Di Benedetto
GitHub: [DibeMateo](https://github.com/DibeMateo)
Proyecto final del curso de Python — Coderhouse.
