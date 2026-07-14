# Guess Game com Kubernetes

## Descrição

Reimplementação da Atividade I utilizando Kubernetes (k3d).

A aplicação é composta por:

- Frontend React
- Backend Flask
- PostgreSQL
- PersistentVolumeClaim
- Horizontal Pod Autoscaler (HPA)

---

## Arquitetura

                 Browser
                     │
                     ▼
             localhost:8080
                     │
                     ▼
              Frontend Service
                     │
                     ▼
           Frontend Deployment
                     │
                     ▼
              Backend Service
                     │
                     ▼
           Backend Deployment
                     │
                     ▼
          PostgreSQL Service
                     │
                     ▼
        PostgreSQL Deployment
                     │
                     ▼
      PersistentVolumeClaim

---

## Pré-requisitos

- Docker
- kubectl
- k3d

---

## Criar o cluster

```bash
k3d cluster create lab
