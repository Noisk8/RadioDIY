# Documentación del Endpoint de Icecast en Python

## Descripción General
Este es un servidor API REST desarrollado en Python utilizando Flask que actúa como intermediario para obtener estadísticas de un servidor Icecast. El servidor proporciona endpoints para obtener información sobre la transmisión de radio, incluyendo oyentes actuales y metadata de la canción en reproducción.

## Tecnologías Utilizadas
- **Flask**: Framework web para Python
- **Flask-CORS**: Extensión para manejar Cross-Origin Resource Sharing
- **Requests**: Librería para realizar peticiones HTTP
- **Python-dotenv**: Para manejar variables de entorno

## Configuración
El servidor utiliza variables de entorno para su configuración:
```env
ICECAST_HOST=http://localhost:8001  # URL del servidor Icecast
```

## Estructura del Proyecto
RADIOVOLKETASERVER/
├── app.py              # Archivo principal con la implementación del servidor Flask
├── icecast.xml         # Archivo de configuración del servidor Icecast
├── .env                # Variables de entorno (ICECAST_HOST)
├── requirements.txt    # Dependencias del proyecto
├── setup.sh           # Script de configuración
└── venv/              # Entorno virtual de Python

### Descripción de los Archivos

#### `app.py`
El archivo principal que contiene toda la lógica del servidor. Incluye:
- Configuración de Flask y CORS
- Implementación de los endpoints
- Función principal para obtener estadísticas
- Manejo de errores y formateo de respuestas

#### `icecast.xml`
Archivo de configuración del servidor Icecast que define:
- Configuración del servidor de streaming
- Puntos de montaje
- Límites y permisos
- Configuración de logs

#### `.env`
Archivo de variables de entorno que contiene:
```env
ICECAST_HOST=http://localhost:8001
```

#### `requirements.txt`
Lista de dependencias del proyecto con versiones específicas:
```
Flask==3.0.0
requests==2.31.0
python-dotenv==1.0.0
flask-cors==4.0.0
```

#### `setup.sh`
Script de shell para la configuración inicial del proyecto que:
- Configura el entorno virtual
- Instala las dependencias
- Establece las variables de entorno necesarias

## Flujo de Datos
1. El cliente realiza una petición HTTP a uno de los endpoints (`/status.json` o `/debug`)
2. El servidor Flask procesa la petición
3. Se realiza una consulta al servidor Icecast configurado en `ICECAST_HOST`
4. Se procesa la respuesta del servidor Icecast
5. Se formatea y devuelve la información al cliente

## Endpoints

### 1. `/status.json`
**Método**: GET  
**Descripción**: Retorna estadísticas procesadas del servidor Icecast.

**Respuesta**:
```json
{
    "oyentes_conectados": 0,
    "cancion_actual": {
        "titulo": "Nombre de la canción",
        "artista": "Nombre del artista"
    },
    "stream_info": {
        "nombre": "Nombre del stream",
        "descripcion": "Descripción del stream",
        "genero": "Género musical",
        "bitrate": "Tasa de bits",
        "url": "URL del stream",
        "tipo": "Tipo de servidor",
        "pico_oyentes": "Pico máximo de oyentes"
    }
}
```

### 2. `/debug`
**Método**: GET  
**Descripción**: Retorna los datos crudos del servidor Icecast sin procesar.

## Funcionalidades Principales

### Función `get_icecast_stats()`
- Realiza una petición al endpoint `status-json.xsl` del servidor Icecast
- Procesa y formatea la información recibida
- Maneja múltiples fuentes de stream
- Separa automáticamente el título y artista si están en formato "Artista - Título"
- Incluye manejo de errores para garantizar una respuesta consistente

## Características de Seguridad y Rendimiento
- Implementación de CORS para permitir peticiones desde diferentes orígenes
- Manejo de errores robusto para evitar fallos del servidor
- Valores por defecto para todos los campos en caso de error
- Configuración mediante variables de entorno para mayor seguridad

## Ejecución
El servidor se ejecuta en el puerto 5000 y acepta conexiones desde cualquier dirección IP:
```python
app.run(host='0.0.0.0', port=5000)
```

## Manejo de Errores
En caso de error, el servidor retorna una respuesta estructurada con:
- Mensaje de error descriptivo
- Valores por defecto para mantener la estructura de la respuesta
- Estado de error claramente identificable

Esta implementación proporciona una capa de abstracción sobre el servidor Icecast, ofreciendo datos procesados y formateados de manera consistente para su fácil consumo por aplicaciones cliente.
