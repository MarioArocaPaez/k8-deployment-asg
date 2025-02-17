# Kubernetes Multi-Service Deployment

Este proyecto despliega una aplicación Flask escalable en Kubernetes, configurando servicios y bases de datos.

## 📌 Requisitos

- Tener un clúster de Kubernetes en ejecución (Minikube, k3s, GKE, etc.).
- Tener `kubectl` instalado y configurado.
- Tener Docker instalado para construir imágenes.

## 📂 Archivos incluidos

- `kubernetes-deployment.yml`: Despliegue de MySQL, Backend, Frontend y Nginx.

## 🚀 Pasos para desplegar

### 1️⃣ Construcción de imágenes Docker

Ejecuta los siguientes comandos para construir las imágenes necesarias:

```sh
cd backend/
docker build -t flasker/api:latest .

cd ../frontend/
docker build -t flasker/frontend:latest .
```

### 2️⃣ Aplicar despliegue en Kubernetes

Ejecuta el siguiente comando para desplegar los servicios:

```sh
kubectl apply -f kubernetes-deployment.yml
```

### 3️⃣ Verificar los pods y servicios

Asegúrate de que todo esté corriendo correctamente:

```sh
kubectl get pods
kubectl get services
```

### 4️⃣ Configurar MySQL

Accede al pod de MySQL para crear la base de datos y las tablas necesarias:

```sh
kubectl exec -it <nombre-del-pod-mysql> -- sh
mysql -u root -p
```

Ejecuta los siguientes comandos en MySQL:

```sql
CREATE USER 'appuser'@'%' IDENTIFIED BY 'apppassword';
GRANT ALL PRIVILEGES ON *.* TO 'appuser'@'%';
CREATE DATABASE test_db;
USE test_db;
CREATE TABLE message (
  id INT AUTO_INCREMENT PRIMARY KEY,
  text VARCHAR(255) NOT NULL
);
INSERT INTO message (text) VALUES ('hola mundo');
```

### 5️⃣ Acceder a la aplicación

Encuentra la IP del nodo y accede en el navegador:

```sh
minikube ip
```

Abre en el navegador:

```
http://<NODE_IP>:30000
```

### 📌 Verificación final

Si todo está correcto, deberías ver el mensaje **"hola mundo"** en la página web.

## 🔥 Comandos útiles

Escalar el backend:

```sh
kubectl scale deployment backend-deployment --replicas=3
```

Eliminar el despliegue:

```sh
kubectl delete -f kubernetes-deployment.yml
