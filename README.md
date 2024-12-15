# Sistema de Cadastro de Clientes - Pessoas

## 📋 Sumário

- [Requisitos](#requisitos)
- [Configuração do Ambiente](#configuração-do-ambiente)
- [Executando o Projeto](#executando-o-projeto)
- [Ambiente de Produção](#ambiente-de-produção)
- [Deployment com Docker](#deployment-com-docker)

## 🚀 Requisitos

- Java JDK
- PostgreSQL
- Docker (opcional)

## ⚙️ Configuração do Ambiente

### Banco de Dados

Você pode configurar o PostgreSQL localmente ou utilizar Docker.

#### Usando Docker para PostgreSQL

```bash
docker run -it --rm \
--name myPostgresDb \
-p 5432:5432 \
-e POSTGRES_USER=postgres \
-e POSTGRES_PASSWORD=postgres \
-e POSTGRES_DB=localdb \
-d postgres
```

## 🏃 Executando o Projeto

### Preparação

Verifique se o arquivo `mvnw` possui permissão de execução. No Linux, se necessário:

```bash
chmod +x mvnw
```

### Executando o Projeto

#### Linux

```bash
./mvnw clean package payara-micro:start
```

> Para pular os testes: `./mvnw clean package -DskipTests=true`

#### Windows

```bash
mvnw.cmd clean package payara-micro:start
```

> **Nota**: Pode ser necessário configurar a variável JAVA_HOME:

```bash
set JAVA_HOME=C:\Program Files\Java\jdk-21
```

Após a inicialização, acesse: [http://localhost:8080](http://localhost:8080)

## 🌐 Ambiente de Produção

### Variáveis de Ambiente Necessárias

| Variável          | Exemplo                                  | Descrição                  |
| ----------------- | ---------------------------------------- | -------------------------- |
| DATABASE_URL      | jdbc:postgresql://127.0.0.1:5432/localdb | URL de conexão com o banco |
| DATABASE_USERNAME | postgres                                 | Usuário do banco           |
| DATABASE_PASSWORD | postgres                                 | Senha do banco             |

### Métodos de Deploy

#### 1. Execução Direta

```bash
java -jar <payara-micro> Cadastro-Pessoas.war
```

> O arquivo WAR está localizado na pasta `target`

#### 2. Deployment com Docker

1. Construa a imagem:

```bash
./mvnw clean package
docker build -t cadpessoas:v1 .
```

1. Execute o container:

```bash
docker run -it --rm \
-e DATABASE_URL="jdbc:postgresql://<url-do-banco>" \
-e DATABASE_USERNAME="<usuario>" \
-e DATABASE_PASSWORD="<senha>" \
-p 8080:8080 cadpessoas:v1
```

Exemplo completo:

```bash
docker run -it --rm \
-e DATABASE_URL="jdbc:postgresql://192.168.0.110:5432/localdb" \
-e DATABASE_USERNAME="postgres" \
-e DATABASE_PASSWORD="postgres" \
-p 8080:8080 cadpessoas:v1
```

Após a inicialização, acesse: [http://localhost:8080/Cadastro-Pessoas](http://localhost:8080/Cadastro-Pessoas)

### Configuração do PostgreSQL em Produção

```bash
docker run -it --rm \
--name ProductPostgresDb \
-p 5432:5432 \
-e POSTGRES_USER=postgres \
-e POSTGRES_PASSWORD=sudodb \
-e POSTGRES_DB=customerdb \
-e PGDATA=/var/lib/postgresql/data/pgdata \
-v C:/Users/UTFPR/Downloads/dados:/var/lib/postgresql/data \
-d postgres
```

## 📝 Notas Adicionais

- Certifique-se de que todas as variáveis de ambiente estejam configuradas corretamente antes de executar em produção
- Verifique se as portas necessárias (8080, 5432) estão disponíveis antes da execução
