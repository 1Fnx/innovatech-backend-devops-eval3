# Innovatech Chile - Backend & Database (Evaluación Parcial N°3 DevOps)

Este repositorio contiene el código fuente de la API REST (Node.js), la configuración de la base de datos (MySQL) y los flujos de CI/CD para la aplicación "Tienda de Alimentos para Perritos" del caso Innovatech Chile.

## 📁 Estructura del Proyecto

El repositorio está dividido en dos microservicios lógicos y la automatización:

*   **`backend/`**: Contiene la lógica de negocio y la API REST.
    *   `server.js`: Servidor Node.js que gestiona las peticiones (CRUD) y la conexión a la base de datos.
    *   `package.json`: Dependencias del proyecto.
    *   `Dockerfile`: Instrucciones para la contenedorización del entorno Node.js.
*   **`db/`**: Contiene la inicialización de la capa de persistencia.
    *   `init.sql`: Script SQL con la estructura de las tablas y los datos iniciales de los productos.
    *   `Dockerfile`: Imagen base de MySQL con la copia del script de inicialización.
*   **`.github/workflows/`**: 
    *   `cicd-tienda-backend.yml`: Pipeline para el despliegue automático del backend.
    *   `cicd-tienda-db.yml`: Pipeline para el despliegue automático de la base de datos.

## 🚀 Arquitectura y Despliegue

La infraestructura backend se ejecuta bajo una arquitectura orquestada y segura en AWS:

1.  **Ejecución Serverless**: Los contenedores operan en **AWS ECS con Fargate**.
2.  **Seguridad de Red**: El backend se encuentra en una red privada y está protegido por *Security Groups* estrictos. Solo acepta tráfico entrante (puerto 3000) proveniente del Frontend. A su vez, la base de datos (puerto 3306) solo acepta conexiones desde el Backend.
3.  **Autoescalado**: El servicio backend cuenta con una política de *Target Tracking* en ECS. Si el promedio de uso de CPU supera el 50%, Fargate aprovisiona automáticamente nuevas tareas para absorber la carga.

## ⚙️ Integración y Despliegue Continuo (CI/CD)

El ciclo de despliegue está automatizado mediante **GitHub Actions** para garantizar entregas ágiles y seguras. Al fusionar cambios en la rama principal, los workflows se encargan de:

1.  **Autenticación**: Inyección segura de variables (GitHub Secrets) para interactuar con la nube de AWS.
2.  **Construcción y Registro (ECR)**: Creación de las imágenes Docker (tanto para la API como para la DB) y almacenamiento en Amazon Elastic Container Registry.
3.  **Despliegue Continuo (ECS)**: Actualización de la *Task Definition* correspondiente y re-despliegue del servicio en el clúster asegurando que la aplicación vuelva a su estado funcional de manera autónoma post-redeploy.
