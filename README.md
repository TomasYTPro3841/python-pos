# POS System v1.9.0

Sistema de Punto de Venta avanzado desarrollado por **Grupo TST**.

## Novedades v1.9.0

### 🎫 Tarjetas de Pago AUID
- **Sistema de tarjetas prepago/crédito**: Crea tarjetas de pago para clientes
- **Saldo recargable**: Los administradores pueden recargar saldo en las tarjetas
- **Pago con PIN**: Los clientes pagan introduciendo su PIN de 4-6 dígitos
- **Código QR**: Cada tarjeta tiene un QR para escanear y pagar sin escribir el ID
- **Panel del cliente**: Página `/card-panel` donde los clientes ven su saldo y QR
- **Historial de transacciones**: Registro completo de movimientos

### 🖥️ Pago con Tarjeta AUID en Kiosko
- Nuevo método de pago "Tarjeta AUID" en el kiosko
- Escanea el QR de la tarjeta con la cámara
- Introduce el PIN para verificar
- Verificación de saldo antes del pago

### 📝 Copyright Dinámico
- El año del copyright se actualiza automáticamente con JavaScript
- Muestra "Grupo TST" como desarrollador

### 🏷️ Administración de Tarjetas
- Nueva página `/payment-cards` (solo admin)
- Crear, modificar, bloquear y eliminar tarjetas
- Ver historial de transacciones por tarjeta
- Recargar saldo

## Novedades v1.8.0

### 🎫 Sistema de Tickets Persistentes
- Los tickets se guardan automáticamente en la base de datos
- Reimpresión ilimitada desde el panel de manager
- Eliminación de tickets desde manager
- Descarga automática de tickets en ventas normales (no en kiosko)

### 📱 Kiosko Mejorado
- **QR Code**: Muestra un código QR por 10 segundos después de cada venta
- **Auto-recarga**: El kiosko se reinicia automáticamente después de mostrar el QR
- **Desactivable**: El kiosko puede ser desactivado desde configuración
- **GUI pulida**: Interfaz rediseñada con animaciones y mejor UX
- **Logo dinámico**: Muestra el logo personalizado en la interfaz

### ⚙️ Configuración Mejorada
- **Dominio y Puerto**: Configura el dominio para que los clientes vean sus tickets via QR
- **Más personalización de tickets**:
  - Website
  - Email
  - Eslogan
  - Opciones de visualización (NIF, teléfono, dirección)
- **Solo administradores**: La configuración ahora es exclusiva para rol admin

### 🎨 Gestión de Tickets (Manager+)
- Nueva página para ver todos los tickets guardados
- Filtrado por fecha y búsqueda por ID
- Ver detalles completos de cada ticket
- Descargar PDF
- Eliminar tickets

### 🖥️ run_server.bat para Windows
- Script de inicio automático para Windows
- Crea entorno virtual si no existe
- Instala dependencias automáticamente
- Inicia el servidor

## Instalación

### Requisitos
- Python 3.8 o superior
- pip (gestor de paquetes Python)

### Windows
```bash
# Opción 1: Usar el script automático
run_server.bat

# Opción 2: Manual
python -m venv venv
venv\Scripts\activate
pip install -r requirements.txt
python pos_server.py
```

### Linux/Mac
```bash
# Opción 1: Usar el script
chmod +x run_server.sh
./run_server.sh

# Opción 2: Manual
python3 -m venv venv
source venv/bin/activate
pip install -r requirements.txt
python pos_server.py
```

## Acceso por defecto

- **URL**: http://localhost:8080
- **Usuario**: admin
- **Contraseña**: admin

⚠️ **Importante**: Cambia la contraseña del admin después del primer inicio.

## Roles de Usuario

| Rol | Permisos |
|-----|----------|
| **admin** | Acceso completo, configuración del sistema, gestión de tarjetas de pago |
| **manager** | Gestión de productos, tickets, descuentos, impuestos, recarga de tarjetas |
| **user** | Ventas y reportes |
| **kiosk_operator** | Solo modo kiosko asignado |

## Estructura del Proyecto

```
pos_v1.9.0/
├── pos_server.py        # Servidor principal
├── pos_core.py          # Lógica del núcleo
├── config_manager.py    # Gestión de configuración
├── ticket_generator.py  # Generador de tickets PDF/PNG/JPG
├── payment_methods.py   # Métodos de pago
├── requirements.txt     # Dependencias Python
├── run_server.bat       # Script inicio Windows
├── run_server.sh        # Script inicio Linux/Mac
├── config.json          # Configuración del sistema
├── pos.db               # Base de datos SQLite
├── web_module/          # Módulo web Flask
│   ├── __init__.py      # Rutas y APIs
│   ├── static/          # Archivos estáticos
│   └── templates/       # Plantillas HTML
│       ├── base.html
│       ├── dashboard.html
│       ├── kiosk.html
│       ├── tickets.html
│       ├── payment_cards.html   # Gestión de tarjetas (admin)
│       ├── card_panel.html      # Panel del cliente
│       └── ...
├── uploads/             # Logos subidos
└── receipts/            # Tickets generados
```

## Características Principales

### Ventas
- Múltiples métodos de pago (efectivo, tarjeta, transferencia, tarjeta AUID)
- Descuentos por porcentaje o cantidad fija
- Productos por unidad o por peso
- Sistema de AUID para clientes

### Tarjetas de Pago AUID
- Tarjetas prepago o crédito
- PIN de 4-6 dígitos
- Código QR para pago rápido
- Historial de transacciones
- Recarga de saldo

### Kiosko
- Autoservicio completo
- Código de barras integrado
- QR de ticket para cliente
- Pago con tarjeta AUID (escaneo QR + PIN)
- No requiere login (configurable)

### Tickets
- Formatos: 58mm, 80mm, A4 (factura)
- Logo personalizado
- QR para ver online
- Guardado para reimpresión
- Exportación PNG/JPG

### Reportes
- Ventas por fecha
- Exportación CSV
- Estadísticas en dashboard

## Configuración del Dominio para QR

Para que los clientes puedan ver sus tickets escaneando el QR:

1. Ve a **Configuración** (solo admin)
2. En "Servidor y Dominio":
   - **Dominio**: Tu dominio o IP pública (ej: `miempresa.com` o `192.168.1.100`)
   - **Puerto**: El puerto donde corre el servidor (por defecto: 8080)
   - **SSL**: Marca si usas HTTPS

El QR generado apuntará a: `http[s]://[dominio]:[puerto]/ticket/[id]`

## Comandos de Consola

El servidor incluye una consola interactiva:

```
POS> productos          # Listar productos
POS> usuarios           # Listar usuarios
POS> status             # Estado del sistema
POS> kiosk_enable on    # Habilitar kiosko
POS> kiosk_enable off   # Deshabilitar kiosko
POS> tickets            # Contar tickets guardados
POS> help               # Ver todos los comandos
POS> exit               # Cerrar servidor
```

## Soporte

Desarrollado por **Grupo TST**

---

© 2024-2025 Grupo TST - POS System v1.9.0
