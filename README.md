# 🚀 Microsserviços com Spring Cloud

> **Sistema de Gerenciamento de Tarefas com Notificações** - Uma arquitetura de microsserviços robusta e escalável construída com Spring Cloud.

## 📋 Índice

- [🎯 Sobre o Projeto](#-sobre-o-projeto)
- [🏗️ Arquitetura](#️-arquitetura)
- [🛠️ Tecnologias Utilizadas](#️-tecnologias-utilizadas)
- [📁 Estrutura do Projeto](#-estrutura-do-projeto)
- [🚀 Como Executar](#-como-executar)
- [🔧 Configuração](#-configuração)
- [📡 Endpoints da API](#-endpoints-da-api)
- [🐳 Docker](#-docker)
- [📊 Monitoramento](#-monitoramento)
- [🤝 Contribuição](#-contribuição)

## 🎯 Sobre o Projeto

Este projeto demonstra a implementação de uma arquitetura de microsserviços utilizando **Spring Cloud**, criando um sistema distribuído para gerenciamento de tarefas com notificações automáticas. O projeto é um exemplo prático de como construir aplicações escaláveis e resilientes seguindo os princípios de microsserviços.

### ✨ Funcionalidades Principais

- **Gerenciamento de Tarefas**: Criação e persistência de tarefas
- **Sistema de Notificações**: Envio automático de notificações
- **Descoberta de Serviços**: Registro e descoberta automática de microsserviços
- **Configuração Centralizada**: Gerenciamento de configurações distribuídas
- **Comunicação entre Serviços**: Integração via OpenFeign

## 🏗️ Arquitetura

```
┌─────────────────┐    ┌──────────────────┐    ┌─────────────────┐
│   Service Main  │    │ Service Tasks    │    │Service Notif.   │
│   (Config +     │◄──►│   (Porta 8081)   │◄──►│   (Porta 8082)  │
│   Eureka)       │    │                  │    │                 │
│   (Porta 8888)  │    │                  │    │                 │
└─────────────────┘    └──────────────────┘    └─────────────────┘
         │                       │                       │
         │                       │                       │
         └───────────────────────┼───────────────────────┘
                                 │
                    ┌─────────────▼─────────────┐
                    │      H2 Database         │
                    │    (In-Memory)           │
                    └───────────────────────────┘
```

### 🔄 Fluxo de Funcionamento

1. **Service Main** inicia como servidor de configuração e descoberta
2. **Service Tasks** se registra no Eureka e se conecta ao banco H2
3. **Service Notification** se registra no Eureka e aguarda requisições
4. Quando uma tarefa é criada, o sistema automaticamente envia uma notificação

## 🛠️ Tecnologias Utilizadas

### 🎯 **Core Technologies**
- **Java 21** - Linguagem de programação principal
- **Spring Boot 3.5.5** - Framework base para desenvolvimento
- **Spring Cloud 2025.0.0** - Suporte para arquitetura de microsserviços

### ☁️ **Spring Cloud Components**
- **Spring Cloud Config Server** - Configuração centralizada
- **Netflix Eureka Server** - Descoberta e registro de serviços
- **Spring Cloud OpenFeign** - Cliente HTTP declarativo para comunicação entre serviços

### 🗄️ **Persistência e Banco de Dados**
- **Spring Data JPA** - Abstração para acesso a dados
- **H2 Database** - Banco de dados em memória para desenvolvimento

### 🐳 **Containerização e Orquestração**
- **Docker** - Containerização dos serviços
- **Docker Compose** - Orquestração e gerenciamento de containers

### 🛠️ **Ferramentas de Desenvolvimento**
- **Maven** - Gerenciamento de dependências e build
- **Lombok** - Redução de código boilerplate
- **Spring Boot Actuator** - Monitoramento e métricas

## 📁 Estrutura do Projeto

```
microsservicos-spring-cloud/
├── 📁 config-server/           # Configurações dos serviços
├── 📁 service.main/            # Servidor principal (Config + Eureka)
│   ├── 📁 src/main/java/
│   └── 📁 src/main/resources/
├── 📁 service.tasks/           # Serviço de gerenciamento de tarefas
│   ├── 📁 src/main/java/
│   └── 📁 src/main/resources/
├── 📁 service.notification/    # Serviço de notificações
│   ├── 📁 src/main/java/
│   └── 📁 src/main/resources/
├── 🐳 docker-compose.yaml      # Orquestração dos containers
└── 📖 README.md                # Este arquivo
```

## 🚀 Como Executar

### 📋 **Pré-requisitos**
- Java 21 ou superior
- Docker e Docker Compose
- Maven 3.6+

### 🚀 **Execução Rápida com Docker**

```bash
# Clone o repositório
git clone <url-do-repositorio>
cd microsservicos-spring-cloud

# Execute com Docker Compose
docker-compose up --build
```

### 🔧 **Execução Local**

```bash
# 1. Inicie o Service Main (Config + Eureka)
cd service.main
mvn spring-boot:run

# 2. Inicie o Service Notification
cd ../service.notification
mvn spring-boot:run

# 3. Inicie o Service Tasks
cd ../service.tasks
mvn spring-boot:run
```

## 🔧 Configuração

### ⚙️ **Portas dos Serviços**
- **Service Main**: `http://localhost:8888`
- **Service Tasks**: `http://localhost:8081`
- **Service Notification**: `http://localhost:8082`

### 🔍 **Acessos**
- **Eureka Dashboard**: `http://localhost:8888/eureka`
- **Health Check**: `http://localhost:8888/actuator/health`

## 📡 Endpoints da API

### 📝 **Service Tasks** (`/tasks`)
```http
POST /tasks
Content-Type: application/json

{
  "title": "Nova Tarefa",
  "description": "Descrição da tarefa"
}
```

### 🔔 **Service Notification** (`/notification`)
```http
POST /notification
Content-Type: application/json

{
  "message": "Mensagem de notificação"
}
```

## 🐳 Docker

### 🏗️ **Build dos Containers**
```bash
# Build individual
docker build -t service-main ./service.main
docker build -t service-tasks ./service.tasks
docker build -t service-notification ./service.notification

# Build com Docker Compose
docker-compose build
```

### 🚀 **Execução**
```bash
# Iniciar todos os serviços
docker-compose up

# Executar em background
docker-compose up -d

# Parar todos os serviços
docker-compose down
```

## 📊 Monitoramento

### 🔍 **Health Checks**
- Todos os serviços incluem endpoints de health check
- Monitoramento automático via Docker health checks
- Integração com Spring Boot Actuator

### 📈 **Métricas**
- Endpoints de métricas disponíveis em cada serviço
- Monitoramento de status dos serviços via Eureka Dashboard

## 🤝 Contribuição

### 📝 **Como Contribuir**
1. Faça um fork do projeto
2. Crie uma branch para sua feature (`git checkout -b feature/AmazingFeature`)
3. Commit suas mudanças (`git commit -m 'Add some AmazingFeature'`)
4. Push para a branch (`git push origin feature/AmazingFeature`)
5. Abra um Pull Request

### 🐛 **Reportando Bugs**
- Use as issues do GitHub para reportar bugs
- Inclua informações sobre o ambiente e passos para reproduzir

---

## 📚 **Recursos Adicionais**

- [Spring Cloud Documentation](https://spring.io/projects/spring-cloud)
- [Spring Boot Reference](https://docs.spring.io/spring-boot/docs/current/reference/html/)
- [Docker Documentation](https://docs.docker.com/)

## 📄 **Licença**

Este projeto está sob a licença MIT. Veja o arquivo `LICENSE` para mais detalhes.

---

<div align="center">

**Desenvolvido com ❤️ por Diogo W.**

[![Java](https://img.shields.io/badge/Java-21-orange.svg)](https://openjdk.java.net/)
[![Spring Boot](https://img.shields.io/badge/Spring%20Boot-3.5.5-brightgreen.svg)](https://spring.io/projects/spring-boot)
[![Spring Cloud](https://img.shields.io/badge/Spring%20Cloud-2025.0.0-blue.svg)](https://spring.io/projects/spring-cloud)
[![Docker](https://img.shields.io/badge/Docker-✓-blue.svg)](https://www.docker.com/)

</div>
