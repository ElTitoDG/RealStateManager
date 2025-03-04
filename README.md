# Gestión de Incidencias - Aplicación Multiplataforma

Este proyecto es una aplicación multiplataforma desarrollada con **Flutter** y **Appwrite**, diseñada para simplificar la comunicación entre propietarios de pisos y empresas de mantenimiento. Los propietarios pueden reportar incidencias (como problemas con persianas o duchas) y las empresas pueden gestionar estas incidencias, proporcionando respuestas y actualizando su estado.

---

## Características Principales
### Para Propietarios:
- Registro e inicio de sesión.
- Creación de incidencias con descripción, fotos y nivel de prioridad.
- Consulta del estado de sus incidencias.
- Recepción de notificaciones en tiempo real.

### Para Empresas de Mantenimiento:
- Registro e inicio de sesión.
- Visualización de las incidencias reportadas por los propietarios.
- Respuesta a incidencias y actualización de su estado.
- Notificaciones en tiempo real cuando se crea o actualiza una incidencia.

---

## Tecnologías Utilizadas
### Frontend:
- **Flutter**: Desarrollo multiplataforma eficiente con una interfaz nativa.
- **Dart**: Lenguaje de programación optimizado para aplicaciones móviles.

### Backend:
- **Appwrite**: Plataforma para gestionar autenticación, base de datos, almacenamiento de archivos y notificaciones en tiempo real.

### Infraestructura:
- **Docker**: Para el despliegue de Appwrite localmente o en la nube.

---

## Configuración del Proyecto
### 1. Requisitos previos
- Flutter instalado en tu sistema. Puedes seguir la guía de instalación en [flutter.dev](https://flutter.dev/docs/get-started/install).
- Docker instalado (si planeas usar Appwrite localmente).

### 2. Instalación
1. Clona este repositorio:
   ```bash
   git clone https://github.com/usuario/RealStateManager.git
   cd RealStateManager
   ```
2. Instala las dependencias:
   ```bash
   flutter pub get
   ```
3. Configura el proyecto en Appwrite:
   - Crea un nuevo proyecto en el panel de control de Appwrite.
   - Configura colecciones para usuarios, empresas e incidencias.
   - Obtén el `project_id` y el endpoint de tu servidor Appwrite.

4. Configura las variables de entorno:
   - Crea un archivo `.env` en la raíz del proyecto y añade:
     ```env
     APPWRITE_ENDPOINT=http://localhost/v1
     APPWRITE_PROJECT_ID=your_project_id
     ```

5. Inicia el proyecto:
   ```bash
   flutter run
   ```

---

## Estructura del Proyecto
```plaintext
├── lib/	            # Código fuente principal de la app.
│   ├── main.dart	    # Archivo principal de la aplicación.
│   ├── components/    # Componentes reutilizables.
│   ├── screens/       # Pantallas principales (login, incidencias, etc.).
│   ├── services/      # Lógica para interactuar con Appwrite.
│   ├── utils/         # Utilidades y helpers.
├── assets/            # Recursos como imágenes y fuentes.
└── README.md          # Este archivo.
```

---

## Ejemplo de Integración con Appwrite
### Registrar una incidencia
```dart
import 'package:appwrite/appwrite.dart';

final client = Client()
  ..setEndpoint('http://localhost/v1')
  ..setProject('your_project_id');

final databases = Databases(client);

await databases.createDocument(
  databaseId: 'DATABASE_ID',
  collectionId: 'COLLECTION_ID',
  documentId: 'unique()',
  data: {
    'descripcion': "Ducha rota",
    'estado': "nueva",
    'propietario_id': "USER_ID",
    'empresa_id': "EMPRESA_ID",
  },
);
```

### Notificaciones en tiempo real
```dart
final realtime = Realtime(client);

realtime.subscribe(['collections.INCIDENCIAS.documents'], (response) {
  print('Notificación: ${response.payload}');
});
```

---

## Próximos Pasos
- Mejorar la interfaz de usuario.
- Añadir soporte para múltiples idiomas.
- Optimizar el sistema de notificaciones.

---

## Contribuciones
¡Las contribuciones son bienvenidas! Por favor, abre un issue o un pull request para cualquier mejora o corrección.

---

## Licencia
Este proyecto está licenciado bajo la [MIT License](LICENSE).