# Meu Teste Docker

Este é um projeto de teste para aprendizado de Docker + Node + Express, com **HTML simples** e organização de volumes via Docker Compose.

---

## Estrutura do projeto

meu-teste-docker/
├── src/
│ └── index.js # Servidor Node
├── public/
│ └── index.html # Página HTML servida pelo Express
├── package.json # Dependências do Node
├── Dockerfile # Definição da imagem
└── docker-compose.yml # Orquestração do container, volumes e portas


---

## Docker Compose e volumes

O Docker Compose foi configurado para:

- Mapear a pasta do projeto no host (`.:/app`) dentro do container, permitindo **hot reload**.  
- Preservar a pasta `node_modules` dentro do container (`/app/node_modules`), para que as dependências instaladas não sejam sobrescritas pelo volume.  
- Mapear a porta **5000 do host** para a porta **3000 do container**, onde o Node está rodando.

### Trecho do `docker-compose.yml`:

```yaml
services:
  app:
    build: .
    container_name: meu-teste-docker-app
    ports:
      - "5000:3000"
    volumes:
      - .:/app
      - /app/node_modules


### Comandos úteis
1. Subir o container e buildar (primeira vez ou rebuild):
    ```bash
	sudo docker-compose up --build
    ```
2. Subir o container sem rebuild:
    ```bash
	sudo docker-compose up
    ```
3. Subir em background:
    ```bash
	sudo docker-compose up -d
    ```
4. Parar e remover containers:
    ```bash
 	sudo docker-compose down
    ```

## Acessar a aplicação no navegador
   ```bash
	http://localhost:5000
    ```

## Diagrama do fluxo do Docker e Node.js

+-------------------+
| Navegador |
| http://localhost:5000

+-------------------+
|
v
+-------------------+
| Host |
| Porta 5000 mapeada|
+-------------------+
|
v
+-------------------+
| Container |
| Porta 3000 (Node) |
| /app |
| ├─ src/index.js |
| ├─ public/ |
| └─ node_modules |
+-------------------+
|
v
+-------------------+
| Servidor Node.js |
| - Responde requisições HTTP
| - Serve arquivos HTML/JS/CSS


**Volumes:**
- `.:/app` → mantém seu código atualizado no container (hot reload)  
- `/app/node_modules` → preserva dependências instaladas, mesmo com o volume  

**Portas:**
- Host 5000 → Container 3000 → Node.js  
- Abra no navegador: `http://localhost:5000`

+-------------------+
# meu-teste-docker
