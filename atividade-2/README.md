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


## Como executar

Clonar o repositório:

```bash
git clone https://github.com/lucasomx/guess-game-docker-compose.git
cd guess-game-docker-compose/atividade-2
```

Criar o cluster k3d, caso ainda não exista:

```bash
k3d cluster create lab
```

Aplicar os manifestos Kubernetes:

```bash
kubectl apply -f k8s/
```

Verificar os recursos:

```bash
kubectl get pods
kubectl get svc
kubectl get pvc
kubectl get hpa
```

Acessar o frontend:

```bash
kubectl port-forward service/frontend 8080:80
```

Em outro terminal, verificar o backend pelo proxy:

```bash
curl http://localhost:8080/api/health
```

Abrir no navegador:

```text
http://localhost:8080
```

Remover os recursos da aplicação:

```bash
kubectl delete -f k8s/
```

Excluir o cluster, caso desejado:

```bash
k3d cluster delete lab
```
