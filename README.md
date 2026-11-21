# Despliegue de MongoDB y Mongo Express en Kubernetes

Este repositorio contiene los manifiestos de Kubernetes necesarios para desplegar una instancia de **MongoDB** y su interfaz web de administración **Mongo Express** en un entorno local (como Minikube).

## Estructura del proyecto

```text
.
├── k8s/
│   ├── mongo-secret.yaml
│   ├── mongo-configmap.yaml
│   ├── mongo-deployment.yaml
│   └── mongo-express-deployment.yaml
├── .gitignore
└── README.md
```

| Archivo | Recurso(s) | Descripción |
| :--- | :--- | :--- |
| `k8s/mongo-secret.yaml` | `Secret` (`mongodb-secret`) | Almacena las credenciales de administrador de MongoDB codificadas en Base64. |
| `k8s/mongo-configmap.yaml` | `ConfigMap` (`mongodb-configmap`) | Define la URL del servicio interno de la base de datos (`mongodb-service`). |
| `k8s/mongo-deployment.yaml` | `Deployment`, `Service` (`ClusterIP`) | Despliega MongoDB en el puerto `27017` y lo expone únicamente dentro del clúster. |
| `k8s/mongo-express-deployment.yaml` | `Deployment`, `Service` (`LoadBalancer`) | Despliega Mongo Express en el puerto `8081` y lo expone al exterior mediante el puerto de nodo `30000`. |

## Requisitos previos

- `kubectl` instalado y configurado.
- Un clúster de Kubernetes local en ejecución (por ejemplo, **Minikube**).

## Despliegue

Aplicar todos los manifiestos con un solo comando:

```bash
kubectl apply -f k8s/
```

O aplicarlos en orden explícito:

```bash
kubectl apply -f k8s/mongo-secret.yaml
kubectl apply -f k8s/mongo-configmap.yaml
kubectl apply -f k8s/mongo-deployment.yaml
kubectl apply -f k8s/mongo-express-deployment.yaml
```

Verificar el estado de los recursos desplegados:

```bash
kubectl get all
```

## Acceso a Mongo Express

En Minikube, exponer y abrir el servicio en el navegador con:

```bash
minikube service mongo-express-service
```

La interfaz web solicita autenticación HTTP básica con las credenciales por defecto de la imagen `mongo-express`:

- **Usuario:** `admin`
- **Contraseña:** `pass`

## Eliminación de recursos

Para eliminar todos los recursos creados en el clúster:

```bash
kubectl delete -f k8s/
```

## Nota de seguridad

El archivo `k8s/mongo-secret.yaml` incluye credenciales de demostración (`username` / `password`) únicamente con fines de aprendizaje. En entornos de producción no se deben versionar secretos en el repositorio; en su lugar se recomienda utilizar herramientas como Sealed Secrets, External Secrets Operator o un gestor externo de secretos.
