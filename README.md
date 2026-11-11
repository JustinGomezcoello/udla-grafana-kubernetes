
# Despliegue de Grafana en Kubernetes

## Universidad de las Américas - UDLA

**Asignatura:** Configuración de clúster  
**Modalidad:** Tarea individual  
**Plataforma:** Grafana  
**Tecnologías:** Docker, Minikube, Kubernetes y kubectl

## 1. Descripción

Este proyecto implementa Grafana en un clúster local
de Kubernetes utilizando Minikube y Docker Desktop.

Se demuestra el funcionamiento de los recursos
Deployment, ReplicaSet, Service y Horizontal Pod
Autoscaler (HPA).

## 2. Objetivos

- Desplegar Grafana en Kubernetes.
- Configurar un Deployment con 2 réplicas.
- Verificar el ReplicaSet generado automáticamente.
- Exponer Grafana mediante un Service NodePort.
- Configurar escalado automático entre 2 y 5 Pods.
- Demostrar la recuperación automática de Pods.

## 3. Tecnologías utilizadas

- Docker Desktop
- Minikube
- Kubernetes
- kubectl
- Grafana OSS
- Metrics Server
- Visual Studio Code

## 4. Arquitectura

Deployment: grafana
  |
  +-- ReplicaSet
       |
       +-- Pod Grafana 1
       +-- Pod Grafana 2

Horizontal Pod Autoscaler:
- Mínimo: 2 Pods
- Máximo: 5 Pods
- Objetivo CPU: 70%

Service NodePort:
- Puerto interno: 3000
- Acceso local mediante Minikube

## 5. Ejecución del proyecto

### Iniciar Kubernetes

```powershell
minikube start --driver=docker
```

### Habilitar métricas

```powershell
minikube addons enable metrics-server
```

### Crear el namespace

```powershell
kubectl create namespace grafana-lab
```

### Desplegar Grafana

```powershell
kubectl apply -f deployment.yaml
kubectl apply -f service.yaml
kubectl apply -f hpa.yaml
```

### Verificar los recursos

```powershell
kubectl get deployments,replicasets,pods,hpa,svc -n grafana-lab
```

### Abrir Grafana

```powershell
minikube service grafana -n grafana-lab
```

## 6. Pruebas realizadas

### Despliegue inicial

Se desplegaron 2 Pods de Grafana mediante
un Deployment y un ReplicaSet.

### Escalado automático

Se configuró un HPA con mínimo 2 y máximo
5 Pods.

Para demostrar su funcionamiento se redujo
temporalmente el objetivo de CPU al 1%
utilizando pruebas/hpa-prueba.yaml.

El HPA incrementó automáticamente las réplicas
hasta 5 Pods.

Posteriormente se restauró el objetivo del 70%,
observando la reducción automática a 2 Pods.

### Recuperación automática

Se eliminó manualmente uno de los Pods.

El ReplicaSet creó un nuevo Pod automáticamente
para recuperar las 2 réplicas deseadas.

## 7. Resultados

- Deployment funcionando correctamente.
- ReplicaSet con réplicas disponibles.
- Grafana accesible desde el navegador.
- Escalado automático de 2 a 5 Pods.
- Reducción automática de 5 a 2 Pods.
- Recuperación automática de un Pod eliminado.

## 8. Evidencias

Las capturas de pantalla se encuentran en
la carpeta evidencias/.

## 9. Consideraciones

Esta implementación es académica y utiliza
almacenamiento local temporal para Grafana.

Para producción se recomendaría almacenamiento
persistente, una base de datos compartida,
gestión segura de credenciales y alta disponibilidad.

El objetivo temporal del 1% se utilizó únicamente
como simulación del escalado automático; no
representa una prueba de carga real.
