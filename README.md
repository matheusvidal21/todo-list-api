# Backend de uma aplicação de Lista de Tarefas

Este repositório contém o código-fonte para o backend de uma aplicação de lista de tarefas que fornece uma API RESTful para gerenciar tarefas, usuários e outras funcionalidades relacionadas a listas de afazeres.

## 🔧 Endpoints da API

### Usuários (Users)

#### ➕ 1. Criação de Usuário (POST)
- URL: `/users/`
- Descrição: Este endpoint permite criar um novo usuário na aplicação.
- Parâmetros:
  - `userModel`: Objeto contendo as informações do usuário, incluindo `username`, `name` e `password`.
- Resposta de Sucesso (HTTP 201):
  - O usuário é criado com sucesso e os detalhes do usuário são retornados no corpo da resposta.
- Resposta de Erro (HTTP 400):
  - Se o usuário já existir, uma mensagem de erro informando que o usuário já existe será retornada.

#### Modelo de Dados do Usuário (UserModel)
- `id`: Identificador único do usuário (UUID).
- `username`: Nome de usuário único.
- `name`: Nome do usuário.
- `password`: Senha do usuário (hash).
- `createdAt`: Data e hora de criação do usuário.

**`(POST /users/)`** 
Exemplo de solicitação JSON para criar um novo usuário:
```json
{
  "username": "novousuario",
  "name": "Novo Usuário",
  "password": "senha123"
}
```

### Tarefas (Tasks)

#### ➕ 2. Criação de Tarefa (POST)
- URL: `/tasks/`
- Descrição: Este endpoint permite criar uma nova tarefa na lista de tarefas de um usuário.
- Parâmetros:
  - `taskModel`: Objeto contendo as informações da tarefa, incluindo `title`, `description`, `startAt`, `endAt` e `priority`.
- Resposta de Sucesso (HTTP 200):
  - A tarefa é criada com sucesso e os detalhes da tarefa são retornados no corpo da resposta.
- Resposta de Erro (HTTP 400):
  - Se a data de início ou data de término for anterior à data atual, ou se a data de início for posterior à data de término, uma mensagem de erro será retornada.
  - Se o usuário não tiver permissão para criar a tarefa, uma mensagem de erro será retornada.

**`(POST /tasks/)`** 
Exemplo de solicitação JSON para criar uma nova tarefa:
```json
{
  "title": "Tarefa 1",
  "description": "Descrição da Tarefa 1",
  "startAt": "2023-10-13T08:00:00",
  "endAt": "2023-10-14T12:00:00",
  "priority": "ALTA"
}
```

#### 📋 3. Listagem de Tarefas (GET)
- URL: `/tasks/`
- Descrição: Este endpoint permite listar todas as tarefas de um usuário.
- Parâmetros: Nenhum.
- Resposta de Sucesso (HTTP 200):
  - A lista de tarefas do usuário é retornada no corpo da resposta.

#### 🔄 4. Atualização de Tarefa (PUT)
- URL: `/tasks/{id}`
- Descrição: Este endpoint permite atualizar uma tarefa existente.
- Parâmetros:
  - `id`: Identificador único da tarefa (UUID) a ser atualizada.
  - `taskModel`: Objeto contendo as informações atualizadas da tarefa.
- Resposta de Sucesso (HTTP 200):
  - A tarefa é atualizada com sucesso e os detalhes da tarefa atualizada são retornados no corpo da resposta.
- Resposta de Erro (HTTP 400):
  - Se a tarefa não for encontrada, uma mensagem de erro será retornada.
  - Se o usuário não tiver permissão para atualizar a tarefa, uma mensagem de erro será retornada.
 
**`(PUT /tasks/{id})`** 
Exemplo de solicitação JSON para atualizar uma tarefa existente:
```json
{
  "title": "Tarefa Atualizada",
  "description": "Descrição atualizada da Tarefa",
  "startAt": "2023-10-15T10:00:00",
  "endAt": "2023-10-16T15:00:00",
  "priority": "MÉDIA"
}
```

#### Modelo de Dados da Tarefa (TaskModel)
- `id`: Identificador único da tarefa (UUID).
- `description`: Descrição da tarefa.
- `title`: Título da tarefa (máximo de 50 caracteres).
- `startAt`: Data e hora de início da tarefa.
- `endAt`: Data e hora de término da tarefa.
- `priority`: Prioridade da tarefa.
- `idUser`: Identificador único do usuário ao qual a tarefa pertence.
- `createdAt`: Data e hora de criação da tarefa.

## 🔑 Autenticação e Autorização

A autenticação de usuários é realizada através do cabeçalho "Authorization" usando o esquema "Basic". A senha do usuário é verificada usando o algoritmo de hash BCrypt.

O filtro `FilterTaskAuth` é responsável por autenticar os usuários ao acessarem endpoints relacionados a tarefas. Se a autenticação for bem-sucedida, o filtro adiciona o ID do usuário à solicitação, permitindo que o usuário acesse as operações relacionadas a tarefas.

## 🚨 Tratamento de Erros

O controlador `ExceptionHandlerController` trata erros relacionados à validação dos dados da solicitação, como datas inválidas e mensagens não legíveis, retornando mensagens de erro apropriadas.

Esse é um resumo dos principais endpoints e funcionalidades do backend da aplicação de lista de tarefas. Certifique-se de configurar e implantar o backend corretamente para utilizá-lo em conjunto com o frontend ou outras partes da aplicação.
