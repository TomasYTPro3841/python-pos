# Sistema POS - Punto de Venta

Un sistema completo de Punto de Venta con múltiples interfaces: consola, GUI de escritorio y web.

## Características

### Funcionalidades Principales
- **Gestión de Productos**: CRUD completo de productos con código, nombre, precio y stock
- **Sistema de Ventas**: Carrito de compras con descuentos (porcentaje o importe fijo)
- **Múltiples Métodos de Pago**: Efectivo, tarjeta y transferencia
- **Tickets Personalizables**: Formato A4 (factura), ticket 58mm y 80mm
- **Exportación**: Tickets en PDF, PNG o JPG manteniendo dimensiones
- **Reportes**: Ventas por fecha con exportación a CSV
- **Sistema de Usuarios**: Con roles (admin, manager, user) y autenticación

### Interfaces
1. **Consola** (`pos_core.py`): Interfaz de línea de comandos
2. **GUI de Escritorio** (`pos_gui.py`): Interfaz gráfica con tkinter
3. **Web** (`web_module/`): Interfaz web con Flask

## Instalación

### Requisitos
- Python 3.8+
- Dependencias listadas en `requirements.txt`

### Pasos

```bash
# 1. Instalar dependencias
pip install -r requirements.txt

# 2. Iniciar el servidor
python pos_server.py
```

### Dependencias Adicionales para Exportación de Imágenes
Para exportar tickets como imagen (PNG/JPG), necesitas instalar poppler:
- **Ubuntu/Debian**: `sudo apt-get install poppler-utils`
- **macOS**: `brew install poppler`
- **Windows**: Descargar desde https://github.com/oschwartz10612/poppler-windows/releases

## Uso

### Iniciar el Servidor

```bash
python pos_server.py
```

El servidor iniciará:
- Servidor Socket en puerto 9999
- Servidor Web en puerto 8080

### Consola de Comandos del Servidor

Una vez iniciado el servidor, puedes usar comandos:

```
POS> help                    # Mostrar ayuda
POS> productos               # Listar productos
POS> add_prod COD Nombre 10.50 100   # Añadir producto
POS> del_prod COD            # Eliminar producto
POS> usuarios                # Listar usuarios
POS> add_user usuario pass admin     # Crear usuario
POS> del_user usuario        # Eliminar usuario
POS> status                  # Estado del servidor
POS> config                  # Ver configuración
POS> exit                    # Cerrar servidor
```

### Cliente GUI

```bash
python pos_gui.py
```

Credenciales por defecto:
- Usuario: `admin`
- Contraseña: `admin`

### Interfaz Web

Accede a `http://localhost:8080` desde cualquier navegador.

## Configuración

El archivo `config.json` contiene toda la configuración:

### Formato de Tickets
```json
{
  "ticket": {
    "format": "ticket_80mm",
    "formats_available": {
      "a4_invoice": {...},
      "ticket_58mm": {...},
      "ticket_80mm": {...}
    },
    "company_info": {
      "name": "Mi Empresa S.A.",
      "address": "Calle Principal 123",
      "nif": "B12345678"
    }
  }
}
```

### Métodos de Pago
```json
{
  "payment_methods": {
    "enabled": ["cash", "card", "transfer"],
    "default": "cash"
  }
}
```

## Estructura del Proyecto

```
pos/
├── pos_server.py        # Servidor principal (socket + web + consola)
├── pos_core.py          # Lógica del POS y base de datos
├── pos_gui.py           # Cliente GUI con tkinter
├── config_manager.py    # Gestor de configuración
├── ticket_generator.py  # Generador de tickets PDF
├── payment_methods.py   # Métodos de pago
├── users.py             # Gestión de usuarios
├── config.json          # Archivo de configuración
├── requirements.txt     # Dependencias
├── web_module/          # Módulo web Flask
│   ├── __init__.py      # App Flask
│   └── templates/       # Plantillas HTML
│       ├── base.html
│       ├── login.html
│       ├── dashboard.html
│       ├── sales.html
│       ├── products.html
│       ├── reports.html
│       ├── users.html
│       └── settings.html
├── receipts/            # Directorio de tickets generados
└── pos.db               # Base de datos SQLite
```

## API REST

### Autenticación
- `POST /login` - Iniciar sesión
- `GET /logout` - Cerrar sesión

### Productos
- `GET /api/products` - Listar productos
- `GET /api/products/<code>` - Obtener producto
- `POST /api/products` - Crear producto (admin)
- `PUT /api/products/<code>` - Actualizar producto (admin)
- `DELETE /api/products/<code>` - Eliminar producto (admin)

### Ventas
- `POST /api/sales` - Procesar venta
- `GET /api/sales/<id>` - Obtener venta
- `GET /api/sales/report` - Reporte por fecha
- `GET /api/sales/export` - Exportar CSV

### Tickets
- `GET /api/tickets/<id>` - Descargar ticket PDF
- `GET /api/tickets/<id>/export?format=png` - Exportar como imagen

### Usuarios (admin)
- `GET /api/users` - Listar usuarios
- `POST /api/users` - Crear usuario
- `DELETE /api/users/<username>` - Eliminar usuario

### Configuración (admin)
- `GET /api/config` - Obtener configuración
- `PUT /api/config` - Actualizar configuración

## Roles de Usuario

- **admin**: Acceso completo a todas las funciones
- **manager**: Acceso a ventas y reportes
- **user**: Solo ventas

## Licencia

¿Qué es eso?
