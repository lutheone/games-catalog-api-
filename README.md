# Games List - API de Catálogo de Jogos

API REST para gerenciar um catálogo de videogames. Permite criar listas personalizadas, adicionar jogos e organizar uma coleção de forma simples e eficiente.

## 🎮 Descrição

Plataforma backend para aplicações que precisam de um catálogo de jogos organizado. Suporta criação de listas temáticas (favoritos, wishlist, jogados, etc) e gerencimento completo de títulos.

## Stacks

- **Java 17** - Linguagem
- **Spring Boot 3.x** - Web framework
- **Spring Data JPA** - Persistência
- **Hibernate** - ORM
- **H2 Database** - Ambiente dev
- **Maven** - Build

## Quickstart

```bash
git clone https://github.com/lutheone/games-list.git
cd games-list
mvn spring-boot:run
```

API disponível em: `http://localhost:8080`

## Endpoints

### Jogos

#### Listar todos os jogos
```
GET /api/games
```

**Response (200)**
```json
[
  {
    "id": 1,
    "title": "Elden Ring",
    "genre": "Action RPG",
    "platform": "PlayStation 5",
    "releaseYear": 2022,
    "rating": 9.5
  },
  {
    "id": 2,
    "title": "The Last of Us",
    "genre": "Action Adventure",
    "platform": "PlayStation 5",
    "releaseYear": 2023,
    "rating": 9.0
  }
]
```

#### Buscar jogo por ID
```
GET /api/games/{id}
```

#### Criar novo jogo
```
POST /api/games
Content-Type: application/json

{
  "title": "Baldur's Gate 3",
  "genre": "RPG",
  "platform": "PC",
  "releaseYear": 2023,
  "rating": 9.8
}
```

**Response (201)**
```json
{
  "id": 3,
  "title": "Baldur's Gate 3",
  "genre": "RPG",
  "platform": "PC",
  "releaseYear": 2023,
  "rating": 9.8
}
```

#### Atualizar jogo
```
PUT /api/games/{id}
Content-Type: application/json

{
  "title": "Baldur's Gate 3",
  "rating": 10.0
}
```

#### Deletar jogo
```
DELETE /api/games/{id}
```

### Listas

#### Listar todas as coleções
```
GET /api/game-lists
```

#### Criar nova lista
```
POST /api/game-lists
Content-Type: application/json

{
  "name": "Meus Favoritos",
  "description": "Jogos que mais curto"
}
```

#### Adicionar jogo à lista
```
POST /api/game-lists/{listId}/games/{gameId}
```

#### Remover jogo da lista
```
DELETE /api/game-lists/{listId}/games/{gameId}
```

## Arquitetura

Segue o padrão **MVC em Camadas**:

```
src/main/java/com/devsuperior/
├── controller/
│   ├── GameController
│   └── GameListController
├── dto/
│   ├── GameDTO
│   └── GameListDTO
├── entity/
│   ├── Game
│   └── GameList
├── repository/
│   ├── GameRepository
│   └── GameListRepository
├── service/
│   ├── GameService
│   └── GameListService
└── exception/
    └── ResourceNotFoundException
```

## Modelo de Dados

### Tabela: games
```sql
CREATE TABLE games (
  id BIGINT PRIMARY KEY AUTO_INCREMENT,
  title VARCHAR(255) NOT NULL,
  genre VARCHAR(100),
  platform VARCHAR(100),
  release_year INT,
  rating DECIMAL(3,1),
  created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```

### Tabela: game_lists
```sql
CREATE TABLE game_lists (
  id BIGINT PRIMARY KEY AUTO_INCREMENT,
  name VARCHAR(255) NOT NULL,
  description TEXT,
  created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```

### Tabela: game_list_items (Relacionamento)
```sql
CREATE TABLE game_list_items (
  game_list_id BIGINT NOT NULL,
  game_id BIGINT NOT NULL,
  PRIMARY KEY (game_list_id, game_id),
  FOREIGN KEY (game_list_id) REFERENCES game_lists(id),
  FOREIGN KEY (game_id) REFERENCES games(id)
);
```

## Exemplos de Uso

### CURL - Criar jogo
```bash
curl -X POST http://localhost:8080/api/games \
  -H "Content-Type: application/json" \
  -d '{
    "title": "Cyberpunk 2077",
    "genre": "Action RPG",
    "platform": "PC",
    "releaseYear": 2020,
    "rating": 7.5
  }'
```

### CURL - Criar lista e adicionar jogos
```bash
# Criar lista
curl -X POST http://localhost:8080/api/game-lists \
  -H "Content-Type: application/json" \
  -d '{"name": "Wishlist 2024", "description": "Jogos que quero jogar"}'

# Adicionar jogo à lista (supondo listId=1, gameId=1)
curl -X POST http://localhost:8080/api/game-lists/1/games/1
```

### Postman
1. Import collection (ou crie manualmente)
2. Configure base URL: `http://localhost:8080`
3. Teste os endpoints

## 📊 Recursos Demonstrados

✅ Relacionamento ManyToMany (Jogos ↔ Listas)  
✅ CRUD completo de entidades  
✅ Validação de dados  
✅ DTOs para abstração  
✅ Tratamento de exceções  
✅ REST best practices  
✅ Paginação (opcional)  

## ⚡ Performance

- Índices nas chaves primárias e estrangeiras
- Lazy loading para otimização
- Queries eficientes via JPA

## Debug

**Console H2:**
```
http://localhost:8080/h2-console
JDBC URL: jdbc:h2:mem:testdb
```
