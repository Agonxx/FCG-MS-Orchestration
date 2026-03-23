# FCG-MS-Orchestration

Repositório de orquestração da plataforma **FIAP Cloud Games**. Centraliza o `docker-compose.yml` e os manifestos Kubernetes de todos os microsserviços.

## Repositórios dos serviços

| Serviço | Repositório | Porta |
|---|---|---|
| UsersAPI | FCG-MS-UsersAPI | 5001 |
| CatalogAPI | FCG-MS-CatalogAPI | 5002 |
| PaymentsAPI | FCG-MS-PaymentsAPI | 5003 |
| NotificationsAPI | FCG-MS-NotificationsAPI | 5004 |

---

## Pré-requisitos

- [Docker Desktop](https://www.docker.com/products/docker-desktop/) com os 4 repos clonados como pastas irmãs:

```
Nova pasta/Testes/
├── FCG-MS-Orchestration/   ← este repo
├── FCG-MS-UsersAPI/
├── FCG-MS-CatalogAPI/
├── FCG-MS-PaymentsAPI/
└── FCG-MS-NotificationsAPI/
```

---

## Subir tudo com Docker Compose

```bash
cd FCG-MS-Orchestration
docker-compose up --build
```

Isso irá:
1. Subir SQL Server e RabbitMQ
2. Aguardar healthcheck de ambos
3. Buildar e subir os 4 microsserviços automaticamente

### Serviços disponíveis após subir

| URL | Descrição |
|---|---|
| `http://localhost:5001/swagger` | UsersAPI — Swagger |
| `http://localhost:5002/swagger` | CatalogAPI — Swagger |
| `http://localhost:5003/swagger` | PaymentsAPI — Swagger |
| `http://localhost:15672` | RabbitMQ Management (guest/guest) |

---

## Deploy no Kubernetes local

### Pré-requisito
Docker Desktop com Kubernetes habilitado + kubectl instalado.

```bash
# 1. Criar namespace
kubectl apply -f k8s/namespace.yaml

# 2. Aplicar todos os manifestos
kubectl apply -f k8s/

# 3. Verificar pods
kubectl get pods -n fiapcloudgames

# 4. Ver logs de um serviço
kubectl logs -l app=usersapi -n fiapcloudgames
```

---

## Estrutura do repositório

```
FCG-MS-Orchestration/
├── docker-compose.yml
└── k8s/
    ├── namespace.yaml
    ├── usersapi-deployment.yaml
    ├── usersapi-service.yaml
    ├── usersapi-configmap.yaml
    ├── usersapi-secret.yaml
    ├── catalogapi-deployment.yaml
    ├── catalogapi-service.yaml
    ├── catalogapi-configmap.yaml
    ├── catalogapi-secret.yaml
    ├── paymentsapi-deployment.yaml
    ├── paymentsapi-service.yaml
    ├── paymentsapi-configmap.yaml
    ├── paymentsapi-secret.yaml
    ├── notificationsapi-deployment.yaml
    ├── notificationsapi-service.yaml
    ├── notificationsapi-configmap.yaml
    └── notificationsapi-secret.yaml
```
