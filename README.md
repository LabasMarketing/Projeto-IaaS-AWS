# ☁️ Projeto IaaS - Lista de Mercado com Infraestrutura AWS

Projeto desenvolvido com foco em **containerização, arquitetura em nuvem e deploy em AWS**, utilizando uma aplicação full-stack de **lista de mercado**, com **back-end em Java Spring Boot** e **front-end em HTML, CSS e JavaScript puro**, executados com **Docker**. No ambiente em nuvem, a solução foi organizada em uma **VPC com subnet pública e subnet privada**, hospedando instâncias EC2 separadas para front-end e back-end. 

## 👨‍💻 Desenvolvedores

- José Pedro Bitetti  
- [Gustavo Netto](https://github.com/gustavonc05)  
- Gabriel Labarca Del Bianco  

---

## 🧭 Visão Geral

A aplicação permite ao usuário:

- Adicionar itens à lista de mercado
- Marcar itens como concluídos
- Remover itens da lista

O projeto foi estruturado para separar claramente as responsabilidades entre interface e API, além de aplicar conceitos de infraestrutura em nuvem com isolamento de rede.

---

## 🛠️ Etapas do Desenvolvimento

### ☁️ 1. Infraestrutura AWS

A infraestrutura foi planejada dentro de uma **VPC** com faixa `10.0.0.0/16`, contendo uma **subnet pública** e subnets privadas, com uso de **Internet Gateway**, **NAT Gateway** e tabelas de rota separadas. No ambiente, uma instância EC2 da rede pública foi utilizada para o **front-end**, enquanto a instância EC2 na rede privada foi utilizada para o **back-end**. O repositório também documenta a presença de grupo de segurança e o isolamento da máquina privada.
#### 📌 Organização da rede
<img width="1226" height="419" alt="Design sem nome" src="https://github.com/user-attachments/assets/9c07a8ed-ad53-4c91-85b8-aae8fef13a5d" />

---- 
| Componente                | Tipo / Tecnologia        | Detalhes |
|--------------------------|--------------------------|----------|
| VPC                      | Rede AWS                 | 10.0.0.0/16 |
| Subnet Pública 1         | AWS Subnet               | 10.0.0.0/24 (AZ A) |
| Subnet Privada 1         | AWS Subnet               | 10.0.1.0/24 (AZ A) |
| Internet Gateway         | Gateway                  | Acesso externo (0.0.0.0/0) |
| NAT Gateway              | Gateway                  | Saída da rede privada |
| Tabela de Rota Pública   | Routing                  | Internet Gateway |
| Tabela de Rota Privada   | Routing                  | NAT Gateway |
| Servidor Web 1           | EC2                      | Subnet pública |
| Servidor Web 2           | EC2                      | Subnet privada |
| Grupo de Segurança       | Firewall AWS             | Protege instância IIS |

---

---

### 🧱 2. Arquitetura dos Componentes

O projeto está estruturado da seguinte forma no repositório: um `docker-compose.yml` na raiz orquestra os serviços, o diretório `backend` concentra a API REST em Spring Boot, e o diretório `frontend` concentra a interface web servida por Nginx.

```text
ProjectCaaS/
├── docker-compose.yml
├── backend/
│   ├── Dockerfile
│   ├── pom.xml
│   └── src/main/java/com/projectcaas/todo/
│       ├── TodoApplication.java
│       ├── model/TodoItem.java
│       ├── repository/TodoItemRepository.java
│       └── controller/TodoItemController.java
├── frontend/
│   ├── Dockerfile
│   ├── nginx.conf
│   └── static/
│       ├── index.html
│       ├── css/styles.css
│       └── js/app.js
```
| Componente | Tecnologia | Porta |
|------------|-----------|-------|
| Backend    | Java 21 + Spring Boot 3.4 | 25000 |
| Frontend   | HTML/CSS/JS + Nginx | 80 |
| Banco de dados | H2 (em memória) | — |

### ⚙️ 3. Back-end

O back-end foi desenvolvido em **Java 21 com Spring Boot 3.4**, utilizando **Spring Data JPA** e banco **H2 em memória**. A API REST é responsável por manipular os itens da lista de mercado, permitindo operações de criação, leitura, atualização e remoção (CRUD).

#### 📌 Estrutura do back-end

- `TodoApplication.java`: classe principal da aplicação
- `TodoItem.java`: entidade JPA
- `TodoItemRepository.java`: repositório de dados
- `TodoItemController.java`: controller REST com os endpoints da aplicação

---

### 💻 4. Front-end

O front-end foi desenvolvido utilizando **HTML5, CSS3 e JavaScript puro**, sendo servido por um **servidor Nginx** dentro de um container Docker. O Nginx também atua como intermediário (proxy) para as requisições da API.

#### 📌 Estrutura do front-end

- `index.html`: interface principal da aplicação
- `css/styles.css`: estilização da página
- `js/app.js`: lógica de interação com a API
- `nginx.conf`: configuração do servidor Nginx

---

### 🐳 5. Containerização com Docker

A aplicação foi estruturada para rodar em containers utilizando **Docker** e **Docker Compose**, permitindo subir front-end e back-end de forma integrada.

### Com Docker Compose (recomendado)

```bash
# Subir a aplicação
docker compose up -d

# Acessar no navegador
# http://localhost

# Parar a aplicação
docker compose down
```

### Sem Docker (desenvolvimento)

```bash
cd backend
mvn spring-boot:run
```

> Neste modo, apenas a API estará disponível em `http://localhost:25000`. O frontend precisa ser servido separadamente.

## API REST

A API está disponível na porta **25000** (acesso direto) ou via proxy Nginx na porta **80** (caminho `/items`).

| Método | Endpoint | Descrição |
|--------|----------|-----------|
| `GET` | `/items` | Listar todos os itens |
| `POST` | `/items` | Criar um novo item |
| `PUT` | `/items/{id}` | Atualizar um item |
| `DELETE` | `/items/{id}` | Remover um item |

---

## 🧠 Aprendizados

Este projeto foi uma excelente oportunidade para:

- Aplicar conceitos de **infraestrutura em nuvem com AWS (VPC, subnets, EC2)**  
- Trabalhar com **Docker e Docker Compose na prática**  
- Separar responsabilidades entre **front-end e back-end em ambientes isolados**  
- Configurar **Nginx como servidor web e proxy reverso**  
- Entender o funcionamento de **redes públicas e privadas com NAT Gateway**  
- Realizar deploy de aplicações reais em ambiente cloud  

---

## 🚀 Como Executar

### 📋 Requisitos

- Docker  
- Docker Compose  
- Git  

---
