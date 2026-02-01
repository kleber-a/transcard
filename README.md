# 🚌 Transcard - Sistema de Gerenciamento de Transporte

![Java](https://img.shields.io/badge/Java-ED8B00?style=for-the-badge&logo=openjdk&logoColor=white)
![Spring Boot](https://img.shields.io/badge/Spring_Boot-6DB33F?style=for-the-badge&logo=spring-boot&logoColor=white)
![Angular](https://img.shields.io/badge/Angular-DD0031?style=for-the-badge&logo=angular&logoColor=white)
![PostgreSQL](https://img.shields.io/badge/PostgreSQL-316192?style=for-the-badge&logo=postgresql&logoColor=white)



O **Transcard** é uma aplicação full-stack desenvolvida como desafio técnico, voltada para gerenciar usuários e seus cartões de transporte.
O projeto segue boas práticas de desenvolvimento, incluindo separação em camadas, uso de **DTOs**, **camadas bem definidas** e migrações de banco de dados, garantindo organização, escalabilidade e manutenção facilitada.

---



https://github.com/user-attachments/assets/82076312-38f2-41b2-b0cf-558cfc094292




#### 📊 Campos das Tabelas

##### **Tabela: `users`**
| Campo       | Tipo         | Restrição                  | Descrição                     |
|------------|-------------|---------------------------|--------------------------------|
| `id`      | BIGSERIAL   | PRIMARY KEY               | Identificador único do usuário |
| `full_name` | VARCHAR(255) | NOT NULL                 | Nome completo do usuário       |
| `email`   | VARCHAR(255) | NOT NULL, UNIQUE         | Email do usuário               |
| `password`| VARCHAR(255) | NOT NULL                 | Senha criptografada            |
| `role`    | VARCHAR(50)  | NOT NULL                 | Papel do usuário (`ADMIN` ou `USER`) |

##### **Tabela: `cards`**
| Campo       | Tipo         | Restrição                  | Descrição                               |
|------------|-------------|---------------------------|----------------------------------------|
| `id`       | BIGSERIAL   | PRIMARY KEY               | Identificador único do cartão          |
| `user_id`  | BIGINT      | NOT NULL, FK → users(id) | Referência ao usuário dono do cartão   |
| `card_number` | BIGINT    | NOT NULL                  | Número do cartão                        |
| `card_name`   | VARCHAR(255) | NOT NULL               | Nome do cartão                          |
| `status`      | BOOLEAN     | NOT NULL, DEFAULT TRUE   | Indica se o cartão está ativo ou inativo |
| `card_type`   | VARCHAR(50) | NOT NULL                | Tipo do cartão (COMUM, ESTUDANTE, TRABALHADOR) |



---


### Modelo de Dados

O projeto possui um **modelo de dados relacional**, onde:

- **Usuários** podem ter múltiplos **cartões** (relação 0:N).   
- Cada **cartão** está associado a exatamente um **usuário**.  
- O banco de dados é gerenciado com **migrações Flyway**, garantindo consistência e versionamento das tabelas.


## 🚀 Funcionalidades

### 👤 Gestão de Usuários e Permissões
* **Perfis de Acesso:** Dois perfis de usuário:
  * **Admin:** pode criar, editar e excluir usuários, além de criar, remover ou alterar o status de cartões de qualquer usuário.  
  * **Usuário comum:** pode editar apenas seu **nome e senha**, consultar seus próprios dados e ativar/inativar seus próprios cartões.
* **Controle de Autorização:** Implementado via Spring Security, com roles `ADMIN` e `USER`.
* **Visualização:** Usuários comuns só têm acesso aos seus dados; admins podem visualizar todos os usuários e cartões.

### 💳 Gestão de Cartões
* Cada usuário pode ter múltiplos cartões associados (`@OneToMany`):
  * **Número do Cartão:** `cardNumber` (Long)  
  * **Nome do Cartão:** `cardName` (String)  
  * **Status:** `status` (Boolean) — ativo ou inativo  
  * **Tipo de Cartão:** `cardType` (COMUM, ESTUDANTE, TRABALHADOR)  
* **Ações disponíveis:**  
  * Admin: criar, remover e alterar status de cartões de qualquer usuário  
  * Usuário comum: ativar ou desativar seus próprios cartões  

### 🔒 Segurança
* Estrutura preparada para autenticação via Spring Security.  
* Roles definidas em enum `Role` para diferenciar admins e usuários comuns.  
* Metodologia baseada em **camadas (Controller, Service, Repository, DTO, Model)** para modularidade e manutenção.  
---

## 🛠️ Stack Tecnológica

### Backend
- **Linguagem:** Java 17+ (Compatível com Java 8)
- **Framework:** Spring Boot 3+
- **Persistência:** Spring Data JPA / Hibernate
- **Banco de Dados:** PostgreSQL
- **Documentação:** Swagger (OpenAPI 3)
- **Migrações:** Flyway
- **Build Tool:** Maven
- **Tratamento de Erros:** Exceções personalizadas e globais com `@ControllerAdvice`

### Frontend
- **Framework:** Angular 18
- **Estilização:** TailwindCSS (Responsivo)
- **Gerenciamento de Estado:** Services & RxJS

---

## 🔗 Repositórios Relacionados

* Backend: [Transcard Backend](https://github.com/kleber-a/transcard_api.git)
* Frontend: [Transcard Frontend](https://github.com/kleber-a/transcard_front.git)

---

<!-- ## 📂 Estrutura do Projeto -->

<!-- ### Backend

```text
transcard/
└── backend/
   ├── entity/       # Entidades de Banco de Dados
   ├── dto/         # Objetos de Transferência (Data Transfer Object)
   ├── repository/  # Interface de comunicação com DB
   ├── service/     # Regras de Negócio e Lógica
   ├── controller/  # APIs REST
   ├── mapper/      # Conversão Entity ↔ DTO
   ├── exceptions/  # Endpoints REST
   └─ infra/ -> Configurações (CORS, Swagger, Security)

```

O frontend está organizado em **components, modulos, pages, services e models**, consumindo a API do backend. -->

## ⚡ Como Executar o Projeto

#### 1️⃣ Rodando com Docker Compose

O projeto pode ser executado facilmente usando **Docker Compose**, sem precisar configurar manualmente o banco de dados ou o backend/frontend.

### Passos

1. Certifique-se de ter **Docker** e **Docker Compose** instalados na sua máquina.

2. No diretório raiz do projeto, rode o comando:

```bash
docker-compose up -d

```

3. Serviços disponíveis:

| Serviço | Porta | URL / Observação |
| :--- | :---: | :---: |
| Postgres | 5432 | Banco de dados |
| Backend | 8080 | http://localhost:8080 |
| Swagger UI | 8080 | http://localhost:8080/swagger-ui.html |
| Frontend | 4200 | http://localhost:4200 |

4. Para parar e remover os containers:

```bash 
docker-compose down
```

### 2️⃣ Rodando Localmente

##### PostgreSQL

1. Instale PostgreSQL localmente.
2. Crie o banco de dados:.

Crie o banco de dados:

```bash
CREATE DATABASE transcard_database;
CREATE USER transcard_user WITH PASSWORD 'transcard_password';
GRANT ALL PRIVILEGES ON DATABASE transcard_database TO transcard_user;

```

##### Back-End
1. Configure o application.properties

```bash
# Configurações do Banco de Dados (PostgreSQL)
spring.datasource.url=jdbc:postgresql://localhost:5432/transcard_database
spring.datasource.username=transcard_user
spring.datasource.password=transcard_password
spring.datasource.driver-class-name=org.postgresql.Driver

# Configurações do JPA / Hibernate
spring.jpa.hibernate.ddl-auto=validate
spring.jpa.database-platform=org.hibernate.dialect.PostgreSQLDialect
# Recomendado: Mostrar o SQL no console para debug (opcional)
# spring.jpa.show-sql=true
```

2. Execute 
```bash
mvn spring-boot:run
```

##### Front-End
1. Entre no diretório do frontend:

```bash 
cd ../frontend
```
2. Instale dependências:
```bash 
npm install
```

3. Configure a URL base da API (no environment):
```bash
export const environment = {
  apiUrl: 'http://localhost:8080'
};
```

4. Execute:
```bash
ng serve
```

