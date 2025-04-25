# Avaliacao-01
ATIVIDADE AVALIATIVA
# Loja API

Este meu projeto é uma API RESTful para gerenciamento de categorias e produtos em uma loja, construída com Spring Boot, Spring Data JPA e MariaDB.

## Estrutura do Projeto

```
projeto-loja-erik/
├── pom.xml              
├── src/
│   ├── main/
│   │   ├── java/com/example/loja/
│   │   │   ├── model/          # Entidades JPA (Categoria, Produto)
│   │   │   ├── repository/     # Repositórios que estendem JpaRepository
│   │   │   ├── controller/     # Endpoints REST CRUD
│   │   │   └── LojaApplication.java  # Classe principal Spring Boot
│   │   └── resources/
│   │       └── application.properties  # Configuração do datasource
│   └── test/                
└── .vscode/                 
```

## Relacionamento entre Entidades

- **Categoria** possui a anotação `@OneToMany(mappedBy = "categoria", cascade = CascadeType.ALL)` para listar todos os produtos associados.
- **Produto** possui a anotação `@ManyToOne` e `@JoinColumn(name = "categoria_id")` para referenciar a categoria a que pertence.

Dessa forma, uma categoria pode conter vários produtos, e cada produto está vinculado a exatamente uma categoria.

## Tecnologias Utilizadas

- Java 17
- Spring Boot 3.1.0
- Spring Data JPA
- MariaDB
- Lombok
- Maven

## Pré-requisitos

- Java 17 ou superior instalado
- Maven instalado
- MariaDB instalado e em execução

## Instalação e Configuração do MariaDB

1. **Atualizar repositórios e instalar MariaDB** (exemplo em Ubuntu):

   ```bash
   sudo apt update
   sudo apt install mariadb-server
   ```
   ([digitalocean.com](https://www.digitalocean.com/community/tutorials/how-to-install-mariadb-on-ubuntu-20-04?utm_source=chatgpt.com), [digitalocean.com](https://www.digitalocean.com/community/tutorials/how-to-install-mariadb-on-ubuntu-22-04?utm_source=chatgpt.com))

2. **Executar o script de segurança** para definir senha de root e remover usuários/ámbitos inseguros:

   ```bash
   sudo mysql_secure_installation
   ```
   ([digitalocean.com](https://www.digitalocean.com/community/tutorials/how-to-install-mariadb-on-ubuntu-20-04?utm_source=chatgpt.com))

3. **Criar o banco de dados** utilizado pela aplicação (exemplo: `erik`):

   ```sql
   CREATE DATABASE erik;
   GRANT ALL PRIVILEGES ON erik.* TO 'root'@'localhost';
   FLUSH PRIVILEGES;
   ```

## Configuração do Projeto

No arquivo `src/main/resources/application.properties`, ajuste conforme necessário:

```properties
spring.datasource.url=jdbc:mariadb://localhost:3306/erik
spring.datasource.username=root
spring.datasource.password=<SUA_SENHA>
spring.jpa.hibernate.ddl-auto=update
spring.jpa.show-sql=true
spring.jpa.properties.hibernate.dialect=org.hibernate.dialect.MariaDBDialect
```

## Executando a Aplicação

No diretório raiz do projeto, execute:

```bash
mvn clean spring-boot:run
```

Ou gere o JAR e execute:

```bash
mvn clean package
java -jar target/loja-0.0.1-SNAPSHOT.jar
```

A API estará disponível em `http://localhost:8080`.

## Testando os Endpoints

### Categorias

- **Listar todas**:  `GET /categorias`
- **Criar nova**:
  ```bash
  curl -X POST http://localhost:8080/categorias \
    -H "Content-Type: application/json" \
    -d '{"nome":"Eletrônicos","descricao":"Dispositivos eletrônicos"}'
  ```
- **Atualizar**: `PUT /categorias/{id}`
- **Deletar**: `DELETE /categorias/{id}`

### Produtos

- **Listar todos**:  `GET /produtos`
- **Criar novo**:
  ```bash
  curl -X POST http://localhost:8080/produtos \
    -H "Content-Type: application/json" \
    -d '{"nome":"Smartphone","preco":1500.0,"descricao":"Celular Android","categoria":{"id":1}}'
  ```
- **Atualizar**: `PUT /produtos/{id}`
- **Deletar**: `DELETE /produtos/{id}`

## Observações

- O relacionamento entre Categoria e Produto garante que ao deletar uma categoria, todos os produtos associados sejam removidos (`cascade = CascadeType.ALL`).
- Use ferramentas como Postman ou Insomnia para facilitar os testes.

