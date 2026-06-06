# Sistema Ficha Treino - MongoDB

```mermaid
erDiagram
    INSTRUTOR ||--o{ FICHA_TREINO : "monta (1:N)"
    FICHA_TREINO }o--o{ EXERCICIO : "contem_exercicios (N:N)"

    INSTRUTOR {
        string id_instrutor PK
        string nome_instrutor
        string cref
    }

    FICHA_TREINO {
        string id_ficha PK
        string nome_ficha
        date data_criacao
    }

    EXERCICIO {
        string id_exercicio PK
        string nome_exercicio
        string grupo_muscular
    }
```

## PowerShell

### Baixar imagem MongoDB para o docker
```bash
docker pull mongodb/mongodb-community-server:latest
```

### Criar container "mongodb"
```bash
docker run --name mongodb -p 27017:27017 -d mongodb/mongodb-community-server:latest
```
##
## Terminal Docker

### Carregar o interpretador de comandos do Mongo
```bash
mongosh --port 27017
```
