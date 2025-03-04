# Plan de Desarrollo de Aplicación con Flutter y Appwrite

## 1. Idea General del Proyecto

La aplicación permitirá a propietarios de pisos (tanto de alquileres normales como turísticos) comunicarse con empresas de mantenimiento para gestionar incidencias.
Los propietarios podrán reportar problemas (como persianas rotas o duchas que no funcionan), y las empresas podrán responder y actualizar el estado de estas incidencias.

---

## 2. Planificación

### Requisitos Funcionales

#### Propietarios:

- Registro e inicio de sesión.
- Registrar incidencias con descripción, fotos y nivel de prioridad.
- Ver respuestas y estado de sus incidencias.

#### Empresas de Mantenimiento:

- Registro e inicio de sesión.
- Ver incidencias de sus clientes.
- Responder y actualizar el estado de las incidencias.

#### Ambos:

- Notificaciones en tiempo real para cambios de estado o nuevas incidencias.

### Requisitos No Funcionales

- **Multiplataforma**: Funcional en Android, iOS y Web.
- **Escalable**: Capaz de manejar múltiples empresas y propietarios.
- **Seguridad**: Roles bien definidos y datos protegidos.

---

## 3. Stack Tecnológico

### Frontend (Aplicación Móvil)

- **Flutter**: Desarrollo multiplataforma eficiente con una interfaz nativa.
- **Dart**: Lenguaje de programación optimizado para aplicaciones móviles.

### Backend (Con Appwrite)

- **Appwrite**: Ofrece autenticación, base de datos, almacenamiento de archivos y notificaciones en tiempo real integradas.
  - **Autenticación**: Registro e inicio de sesión con roles (propietarios o empresas).
  - **Base de Datos**: Document-based, ideal para gestionar incidencias, usuarios y empresas.
  - **Realtime**: Notificaciones en tiempo real de actualizaciones en las incidencias.
  - **Almacenamiento**: Para imágenes de incidencias (por ejemplo, fotos del problema).

---

## 4. Arquitectura de la App

### Usuarios y Roles

- **Propietarios**:
  - Pueden reportar incidencias, ver su estado y recibir notificaciones.
- **Empresas**:
  - Pueden ver las incidencias de sus clientes, actualizarlas y comunicarse con ellos.
- **Roles y Permisos**:
  - Usa el sistema de permisos de Appwrite para diferenciar accesos y acciones entre propietarios y empresas.

### Gestión de Incidencias

- Incidencias tienen los siguientes campos:
  - Descripción.
  - Foto opcional.
  - Estado (nueva, en proceso, resuelta).
  - Fecha de creación y última actualización.
- Relaciones:
  - Propietarios → Empresas → Incidencias.

### Flujo de Notificaciones

- Cuando un propietario crea una incidencia, la empresa asignada recibe una notificación.
- Cuando la empresa responde o actualiza el estado, el propietario recibe una notificación.

---

## 5. Fases del Desarrollo

### Fase 1: Configuración del Proyecto

#### Instalación de Flutter y Configuración Inicial

1. Instala Flutter siguiendo la guía oficial en [flutter.dev](https://flutter.dev/docs/get-started/install).
2. Verifica la instalación con:
   ```bash
   flutter doctor
   ```
3. Crea un nuevo proyecto Flutter:
   ```bash
   flutter create RealStateManager
   cd RealStateManager
   ```
4. Instala las dependencias necesarias en `pubspec.yaml`:
   ```yaml
   dependencies:
     flutter:
       sdk: flutter
     appwrite: ^10.0.0 # Versión más reciente
   ```
5. Ejecuta el proyecto para comprobar que funciona correctamente:
   ```bash
   flutter run
   ```

### Configuración de Appwrite

1. Configura un proyecto en Appwrite.
2. Crea las colecciones necesarias:
   - **Usuarios**: Para almacenar los roles y datos básicos.
   - **Empresas**: Para relacionar propietarios y gestionar incidencias.
   - **Incidencias**: Para almacenar los reportes.

### Fase 2: Autenticación y Roles

- Configura la autenticación con Appwrite.
- Define roles para propietarios y empresas usando el sistema de permisos.

### Fase 3: Gestión de Incidencias

- Implementa un formulario para que los propietarios registren incidencias.
- Usa la API de base de datos de Appwrite para almacenar y listar las incidencias.
- Permite a las empresas actualizar el estado de las incidencias.

### Fase 4: Notificaciones en Tiempo Real

- Configura las **Realtime Subscriptions** de Appwrite para notificar a los usuarios sobre cambios en las incidencias.

### Fase 5: Almacenamiento de Imágenes

- Usa el almacenamiento de Appwrite para permitir a los propietarios subir fotos de los problemas reportados.

### Fase 6: Pruebas y Despliegue

- Prueba en dispositivos reales.
- Despliega Appwrite en la nube o localmente con Docker.

---

## 6. Ejemplo de Integración con Appwrite

### Configuración Inicial:

```dart
import 'package:appwrite/appwrite.dart';

final client = Client()
  ..setEndpoint('http://localhost/v1') // Cambia por tu endpoint
  ..setProject('YOUR_PROJECT_ID');
```

### Registrar una Incidencia:

```dart
import 'package:appwrite/appwrite.dart';

final databases = Databases(client);

databases.createDocument(
  databaseId: 'DATABASE_ID',
  collectionId: 'COLLECTION_ID',
  documentId: 'unique()',
  data: {
    'descripcion': "Ducha rota",
    'estado': "nueva",
    'propietario_id': "USER_ID",
    'empresa_id': "EMPRESA_ID',
  },
);
```

### Suscribirse a Notificaciones:

```dart
final realtime = Realtime(client);

realtime.subscribe(['collections.INCIDENCIAS.documents'], (response) {
  print('Notificación: ${response.payload}');
});
```

