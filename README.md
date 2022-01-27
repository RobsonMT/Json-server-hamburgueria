# [Hamburgueria](https://json-hamburgueria.herokuapp.com/)

## Endpoints

- <a href="#users">/users</a>
- <a href="#products">/products</a>
- <a href="#cart">/cart</a>

<h2 id="users" align ='center'> Cadastro </h2>

`POST /register`
`POST /signup`
`POST /users`

Qualquer um desses 3 endpoints irá cadastrar o usuário na lista de "Users".
Os campos obrigatórios são os de email e password.
Você pode ficar a vontade para adicionar qualquer outra propriedade no corpo do cadastro dos usuários.

`POST /signup - FORMATO DA REQUISIÇÃO`

```json
{
  "name": "Robson",
  "email": "robson@mail.com",
  "password": "123456"
}
```

`POST /signup - FORMATO DA RESPOSTA - 200 Created`

```json
{
  "accessToken": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJlbWFpbCI6InJvYmVydEBnbWFpbC5jb20iLCJpYXQiOjE2NDIwMzgyMTcsImV4cCI6MTY0MjA0MTgxNywic3ViIjoiMSJ9.lJR--iEy4tPaSP31MPAnnOWoKK3aeaJTTkzLWnCkiYg",
  "user": {
    "email": "robson@mail.com",
    "name": "Robson",
    "id": 1
  }
}
```

### Possiveis erros </br>

Caso esteja tentando adicionar um novo usuário com um email que já esteja cadastrado.

`POST /signup - FORMATO DA RESPOSTA - 400 Bad Request`

```json
{
  "name": "JhonDoe",
  "email": "robson@mail.com",
  "password": "123456"
}
```

`Retorno:`

```json
"Email already exists"
```

<h2 align ='center'> Login </h2>

`POST /login`
`POST /signin`

Qualquer um desses 2 endpoints pode ser usado para realizar login.

`POST /signin - FORMATO DA REQUISIÇÃO`

```json
{
  "email": "robson@mail.com",
  "password": "123456"
}
```

`POST /signin - FORMATO DA RESPOSTA - 200 OK`

```json
{
  "accessToken": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJlbWFpbCI6InJvYmVydEBnbWFpbC5jb20iLCJpYXQiOjE2NDIwMzgwNDEsImV4cCI6MTY0MjA0MTY0MSwic3ViIjoiMSJ9.hEyFlcAYVivNsX4OB-bK7yBW-abLzoBfeeVCI4FD1Yg",
  "user": {
    "email": "robson@mail.com",
    "name": "Robson",
    "id": 1
  }
}
```

### Possíveis erros

Caso email ou senha digitados estejam incorretos.

`POST /signin - FORMATO DA REQUISIÇÃO`

```json
{
  "email": "johndoe@mail.com",
  "password": "000404"
}
```

`POST /signin - FORMATO DA RESPOSTA - 400 Bad Request`

Caso email incorreto

```json
"Cannot find user"
```

Caso password incorreto

```json
"Incorrect password"
```

<h2 align ='center'> Atualizar dados do usuário </h2>

E possível atualizar qualquer dado do usuário sendo necessário passar o número do id do usuário no caminho da requisição. Esta rota necessita de autenticação ,todas as rotas que necessitam de autenticação e necessário passar o accessToken no header da requisição.
</br>`Exemplo:`

```json
{
  "headers": {
    "Authorization": "Bearer: ${accessToken}"
  }
}
```

`PATCH /users/1 - FORMATO DA REQUISIÇÃO`

```json
{
  "name": "Robson",
  "email": "robson@mail.com",
  "password": "654321"
}
```

`PATCH /users/1 - FORMATO DA RESPOSTA - 200 OK`

```json
{
  "email": "robson@mail.com",
  "password": "$2a$10$3svpjhnOoDOPpc4fnPYNkuDRIJbGkCPXLxNmne0bqemFy.fErAZUu",
  "name": "Robson",
  "id": 1
}
```

### Possíveis erros

> Caso id não pertença ao usuário. </br>`401 Unauthorized`

```json
"Cannot read properties of undefined (reading 'id')"
```

> Caso id não tenha sido passado na requisição. </br> `403 Forbidden`

```json
"Private resource access: entity must have a reference to the owner id"
```

> Caso acessToken não tenha sido passado na requisição. </br> `401 Unauthorized`

```json
"Missing token"
```

<h2 id="products" align ='center'> Listar produtos </h2>

Lista todos os produtos cadastrados, sendo 26 produtos no total. Não e necessário estar logado para acessar os produtos.

`GET /products - FORMATO DA RESPOSTA - 200 OK`</br>

```json
[
  {
    "name": "Hamburger",
    "price": 12,
    "category": "Sanduíches",
    "image": "https://i.ibb.co/Tbd2CyQ/Hamburger.png"
  },
  {
    "name": "Cheeseburger",
    "price": 14,
    "category": "Sanduíches",
    "image": "https://i.ibb.co/fFfTkbp/churros.png"
  },
  {
    "name": "X-Fiesta",
    "price": 14,
    "category": "Sanduíches",
    "image": "https://i.ibb.co/317jFg0/x-fiesta.png"
  },
  {
    "name": "X-Burger",
    "price": 16,
    "category": "Sanduíches",
    "image": "https://i.ibb.co/9h9sZpr/x-burger.png"
  },
  {
    "name": "Quarteirão",
    "price": 16,
    "category": "Sanduíches",
    "image": "https://i.ibb.co/3S849qj/quarteirao.png"
  }
]
```

<h2 id="cart" align ='center'>Adicionar ao carrinho</h2>

Para adicionar um produto ao carrinho e necessário passar o corpo do produto selecionado adicionando a propriedade "urserId".

`POST /cart - FORMATO DA RESPOSTA - 201 Created`</br>

```json
{
  "name": "Hamburger",
  "price": 12,
  "category": "Sanduíches",
  "image": "https://i.ibb.co/Tbd2CyQ/Hamburger.png",
  "quantity": 1,
  "userId": 1
}
```

<h2 align ='center'>Carrinho</h2>

Listar seus produtos adicionados ao carrinho ,e necessário passar o id do usuário no caminho da requisição.</br>
EX: /users/user:id/cart

`GET /users/1/cart - FORMATO DA RESPOSTA - 201 Created`</br>

```json
[
  {
    "name": "Hamburger",
    "price": 12,
    "category": "Sanduíches",
    "image": "https://i.ibb.co/Tbd2CyQ/Hamburger.png",
    "quantity": 1,
    "userId": 1,
    "id": 1
  }
]
```

<h2 align ='center'>Atualizar quantidade</h2>

Para atualizar a quantidade e necessário passar o id do produto selecionado no caminho da requisição e o atributo "quantity" especificando o valor.

`PATCH/cart/1 - FORMATO DA REQUISIÇÃO - 200 OK`</br>

```json
{
  "quantity": 2
}
```

`PATCH/cart/1 - FORMATO DA RESPOSTA - 200 OK`</br>

```json
{
  "name": "Hamburger",
  "price": 12,
  "category": "Sanduíches",
  "image": "https://i.ibb.co/Tbd2CyQ/Hamburger.png",
  "quantity": 2,
  "userId": 1,
  "id": 1
}
```

<h2 align ='center'>Remover do carrinho</h2>

Para remover um item do carrinho e necessário passar o id do produto selecionado no caminho da requisição.

`DELETE /cart/1 - FORMATO DA RESPOSTA - 200 OK`</br>

```json
{}
```
