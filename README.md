# Sistema Ficha Treino - MongoDB

## Modelo ER

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

## Terminal Docker

### Carregar o interpretador de comandos do Mongo
```bash
mongosh --port 27017
```

## Versão 1: Embedded Relationships

### Criar versão 1
```bash
use versao1
```

### Criar coleção fichas
```bash
db.createCollection("fichas")
```

### Inserir dados
```bash
db.fichas.insert({
	"id_ficha": 1,
	"nome_ficha": "Treino1",
	"data_criacao": "05/06/26",
	"instrutor": {
		"id_instrutor": 1,
		"nome_instrutor": "Daniel",
		"cref": "CREF 000000"
	},
	"exercicios": [
	{
		"id_exercicio": 1,
		"nome_exercicio": "Supino",
		"grupo_muscular": "peito"
	},
	{
		"id_exercicio": 2,
		"nome_exercicio": "Puxada",
		"grupo_muscular": "costa"
	}
	]
})

db.fichas.insert({
	"id_ficha": 2,
	"nome_ficha": "Treino2",
	"data_criacao": "05/06/26",
	"instrutor": {
		"id_instrutor": 1,
		"nome_instrutor": "Daniel",
		"cref": "CREF 000000"
	},
	"exercicios": [
	{
		"id_exercicio": 3,
		"nome_exercicio": "LegPress",
		"grupo_muscular": "perna"
	}
	]
})
```

### Consulta 1 (N:N)
```bash
db.fichas.find(
  { "exercicios.nome_exercicio": "Supino" },
)
```

### Consulta 2 (1:N)
```bash
db.fichas.find(
  { "instrutor.nome_instrutor": "Daniel" },
)
```

## Versão 2: Referenced Relationships

### Criar versão 2
```bash
use versao2
```

### Criar coleções
```bash
db.createCollection("instrutores")
db.createCollection("fichas")
db.createCollection("exercicios")
db.createCollection("exercicios_fichas")
```

### Adicionando dados
```bash
db.instrutores.insert({
	"id_instrutor": 1,
	"nome_instrutor": "Daniel",
	"cref": "CREF 00000"
})

db.fichas.insert({
	"id_ficha": 1,
	"nome_ficha": "Treino1",
	"data_criacao": "06/06/26",
	"id_instrutor": 1
})

db.fichas.insert({
	"id_ficha": 2,
	"nome_ficha": "Treino2",
	"data_criacao": "06/06/26",
	"id_instrutor": 1
})

db.exercicios.insert({
	"id_exercicio": 1,
	"nome_exercicio": "Supino",
	"grupo_muscular": "peito"
})

db.exercicios.insert({
	"id_exercicio": 2,
	"nome_exercicio": "Puxada",
	"grupo_muscular": "costa"
})

db.exercicios.insert({
	"id_exercicio": 3,
	"nome_exercicio": "LegPress",
	"grupo_muscular": "pernas"
})

db.exercicios_fichas.insert({
	"id_exercicios_fichas": 1,
	"id_ficha": 1,
	"id_exercicio": 1
})

db.exercicios_fichas.insert({
	"id_exercicios_fichas": 2,
	"id_ficha": 1,
	"id_exercicio": 2
})

db.exercicios_fichas.insert({
	"id_exercicios_fichas": 3,
	"id_ficha": 2,
	"id_exercicio": 3
})
```

### Consulta 1 (N:N)
```bash
db.fichas.find(
  { "id_instrutor": 1 }
)
```

### Consulta 2 (1:N)
```bash
db.exercicios_fichas.find(
  { "id_exercicio": 1 }
)
```
