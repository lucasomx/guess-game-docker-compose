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



## Como executar localmente

### Pré-requisitos

É necessário possuir:

- Git;
- Docker;
- Docker Compose.

Clonar o repositório:

```bash
git clone https://github.com/lucasomx/guess-game-docker-compose.git
cd guess-game-docker-compose/atividade-1
```

Construir as imagens e iniciar os containers:

```bash
docker compose up --build -d --scale backend=3
```

Verificar os containers:

```bash
docker compose ps
```

Abrir o frontend no navegador:

```text
http://localhost:8080
```

Verificar a comunicação com o backend por meio do proxy reverso:

```bash
curl http://localhost:8080/api/health
```

Para encerrar o ambiente:

```bash
docker compose down
```

## Execução em um host remoto

A aplicação também pode ser executada em uma máquina remota ou em uma
máquina virtual com Docker instalado.

No host remoto:

```bash
git clone https://github.com/lucasomx/guess-game-docker-compose.git
cd guess-game-docker-compose/atividade-1
docker compose up --build -d --scale backend=3
```

A porta TCP `8080` deve estar liberada no firewall do servidor. Em provedores de nuvem, também deve ser criada uma regra de entrada no grupo de segurança ou firewall da instância.

A aplicação poderá ser acessada por:

```text
http://IP_DO_SERVIDOR:8080
```

Por exemplo:

```text
http://192.168.1.50:8080
```

O backend não precisa ser exposto diretamente na porta `5000`, pois o NGINX encaminha internamente as requisições recebidas em `/api`.

A variável abaixo é incorporada ao frontend durante a construção da imagem:

```dockerfile
ENV REACT_APP_BACKEND_URL=/api
```

O uso do caminho relativo `/api` permite que frontend e backend sejam
acessados pelo mesmo endereço, tanto localmente quanto em um host remoto.


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

