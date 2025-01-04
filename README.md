# Gestión de Incidencias - Aplicación Multiplataforma

Este proyecto es una aplicación multiplataforma desarrollada con **React Native** y **Appwrite**, diseñada para simplificar la comunicación entre propietarios de pisos y empresas de mantenimiento. Los propietarios pueden reportar incidencias (como problemas con persianas o duchas) y las empresas pueden gestionar estas incidencias, proporcionando respuestas y actualizando su estado.

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
- **React Native**: Desarrollo multiplataforma eficiente.
- **Expo**: Configuración inicial rápida y herramientas preconfiguradas.
- **TypeScript**: Tipado estático para mayor robustez y escalabilidad.

### Backend:
- **Appwrite**: Plataforma para gestionar autenticación, base de datos, almacenamiento de archivos y notificaciones en tiempo real.

### Infraestructura:
- **Docker**: Para el despliegue de Appwrite localmente o en la nube.

---

## Configuración del Proyecto
### 1. Requisitos previos
- Node.js y npm instalados.
- Docker instalado (si planeas usar Appwrite localmente).
- Expo CLI instalada:
  ```bash
  npm install -g expo-cli
  ```

### 2. Instalación
1. Clona este repositorio:
   ```bash
   git clone https://github.com/usuario/proyecto-gestion-incidencias.git
   cd proyecto-gestion-incidencias
   ```
2. Instala las dependencias:
   ```bash
   npm install
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
   expo start
   ```

---

## Estructura del Proyecto
```plaintext
├── App.tsx            # Archivo principal de la app.
├── components/        # Componentes reutilizables.
├── screens/           # Pantallas principales (login, incidencias, etc.).
├── services/          # Lógica para interactuar con Appwrite.
├── utils/             # Utilidades y helpers.
└── README.md          # Este archivo.
```

---

## Ejemplo de Integración con Appwrite
### Registrar una incidencia
```typescript
import { Databases } from "appwrite";

const databases = new Databases(client);
await databases.createDocument('DATABASE_ID', 'COLLECTION_ID', 'UNIQUE()', {
  descripcion: "Ducha rota",
  estado: "nueva",
  propietario_id: "USER_ID",
  empresa_id: "EMPRESA_ID",
});
```

### Notificaciones en tiempo real
```typescript
client.subscribe('collections.INCIDENCIAS.documents', (response) => {
  console.log('Notificación:', response);
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
