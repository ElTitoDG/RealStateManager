# Plan de Desarrollo de Aplicación con React Native y Appwrite

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
- **React Native**: Desarrollo multiplataforma eficiente.
- **Expo**: Configuración inicial rápida con herramientas preconfiguradas.
- **TypeScript**: Para un código más robusto y escalable.

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
```typescript
import { Client, Account, Databases } from "appwrite";

const client = new Client();
client
  .setEndpoint("http://localhost/v1") // Cambia por tu endpoint
  .setProject("YOUR_PROJECT_ID");
```

### Registrar una Incidencia:
```typescript
import { Databases } from "appwrite";

const databases = new Databases(client);
databases.createDocument('DATABASE_ID', 'COLLECTION_ID', 'UNIQUE()', {
  descripcion: "Ducha rota",
  estado: "nueva",
  propietario_id: "USER_ID",
  empresa_id: "EMPRESA_ID",
});
```

### Suscribirse a Notificaciones:
```typescript
client.subscribe('collections.INCIDENCIAS.documents', (response) => {
  console.log('Notificación:', response);
});
```

---

## Próximos Pasos
¿Quieres ayuda para definir las colecciones en Appwrite, configurar roles y permisos, o diseñar la interfaz de usuario?
