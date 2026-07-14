# Guess Game com Docker Compose


O objetivo foi conteinerizar uma aplicação completa utilizando Docker Compose, composta por um frontend React, um backend Flask, um banco de dados PostgreSQL e um servidor NGINX atuando como proxy reverso.

```bash
Arquitetura da aplicação


                  Browser
                      │
                      ▼
             http://localhost:8080
                      │
                      ▼
                   NGINX
            (Frontend + Proxy)
                      │
          ┌───────────┴───────────┐
          ▼                       ▼
      React SPA             Backend Flask
                            (3 instâncias)
                                  │
                                  ▼
                            PostgreSQL
                           (Volume persistente)

```

Tecnologias utilizadas

- Docker
- Docker Compose
- Python 3.12
- Flask
- React
- Node.js 18
- PostgreSQL 16
- NGINX

```bash
Estrutura do projeto

atividade-1/
├── Dockerfile.backend
├── docker-compose.yml
├── frontend/
│   ├── Dockerfile
│   └── default.conf
├── guess/
├── repository/
├── requirements.txt
├── run.py
└── README.md
```


Componentes

### Backend

Aplicação desenvolvida em Flask responsável pela lógica do jogo de adivinhação.

Características:

- executado em container próprio;
- comunicação com PostgreSQL;
- escalável por múltiplas instâncias.



### Frontend

Aplicação React servida pelo NGINX.

O NGINX também atua como proxy reverso para encaminhar as requisições da API ao backend.



### Banco de Dados

Banco PostgreSQL executado em container independente.

Os dados são armazenados em volume persistente para evitar perda de informações quando os containers são recriados.



## Docker Compose

O projeto utiliza Docker Compose para orquestrar os serviços:

- frontend
- backend
- postgres

O backend utiliza variáveis de ambiente para configuração da conexão com o banco de dados.



## Balanceamento de carga

O NGINX foi configurado como proxy reverso para encaminhar requisições da API para o serviço backend.

O backend pode ser escalado para múltiplas instâncias utilizando:

docker compose up --build -d --scale backend=3




## Persistência de dados

O PostgreSQL utiliza um volume Docker:

postgres_data

permitindo que os dados permaneçam disponíveis mesmo após reinicializações dos containers.



## Como executar

Clonar o repositório:

git clone https://github.com/lucasomx/guess-game-docker-compose.git

cd guess-game-docker-compose/atividade1

Construir e iniciar os containers:

docker compose up --build -d --scale backend=3

Para encerrar o ambiente:

docker compose down


## Testes

Verificar os containers:

docker compose ps

Verificar o proxy reverso e a comunicação com o backend:

curl http://localhost:8080/api/health

Resposta esperada:

status: ok

Verificar o Frontend:

Abrir no navegador:

http://localhost:8080




## Atualização dos componentes

Cada serviço é independente e pode ser atualizado alterando apenas a imagem correspondente:

- Backend
- Frontend
- PostgreSQL

Após alterações:

docker compose up --build -d

reconstrói apenas os serviços modificados.

