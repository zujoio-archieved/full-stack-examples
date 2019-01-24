GraphQL
===

**GraphQL** is a new API standard that provides a more efficient, powerful and flexible alternative to REST. It was developed and open-sourced by Facebook and is now maintained by a large community of companies and individuals from all over the world.

 A GraphQL server only exposes a single endpoint and responds with precisely the data a client asked for.

 - GraphQL - A Query Language for APIs
 - A more efficient Alternative to REST
    1. Increased mobile usage creates need for efficient data loading
    2. Variety of different frontend frameworks and platforms.
    3. Fast development & expectation for rapid feature development.

## GraphQL is the better REST
GraphQL was developed to cope with the need for more flexibility and efficiency! It solves many of the shortcomings and inefficiencies that developers experience when interacting with REST APIs.

An app needs to display the titles of the posts of a specific user. The same screen also displays the names of the last 3 followers of that user. How would that situation be solved with REST and GraphQL?

## Data Fetching with REST vs GraphQL
Endpoints:
---
1. `/users/<id>`
2. `/users/<id>/posts`
3. `/users/<id>/followers`

API:
![api](https://imgur.com/VIWd5I5.png)

GraphQL:
![graphql](https://imgur.com/uY50GHz.png)

## No more Over- and Underfetching
One of the most common problems with REST is that of over- and underfetching.

This happens because the only way for a client to download data is by hitting endpoints that return fixed data structures. 

It’s very difficult to design the API in a way that it’s able to provide clients with their exact data needs.

### Overfetching: Downloading superfluous data
Overfetching means that a client downloads more information than is actually required in the app.

### Underfetching and the n+1 problem
Underfetching generally means that a specific endpoint doesn’t provide enough of the required information.

## Rapid Product Iterations on the Frontend
With every change that is made to the UI, there is a high risk that now there is more (or less) data required than before. 

This kills productivity and notably slows down the ability to incorporate user feedback into a product.

With GraphQL,Since clients can specify their exact data requirements, no backend engineer needs to make adjustments when the design and data needs on the frontend change.

## Insightful Analytics on the Backend
With GraphQL, you can also do low-level performance monitoring of the requests that are processed by your server.

## Benefits of a Schema & Type System
GraphQL uses a strong type system to define the capabilities of an API. All the types that are exposed in an API are written down in a schema using the GraphQL Schema Definition Language (SDL).

This schema serves as the contract between the client and the server to define how a client can access the data.

Core Concepts
===

## The Schema Definition Language (SDL)
GraphQL has its own type system that’s used to define the schema of an API. The syntax for writing schemas is called Schema Definition Language (SDL).

```graphql
type Person {
  name: String!
  age: Int!
}

type Post {
  title: String!
  author: Person!
}

type Person {
  name: String!
  age: Int!
  posts: [Post!]!
}
```

## Fetching Data with Queries
```graphql
{
  allPersons {
    name
  }
}
```
```json
{
  "allPersons": [
    { "name": "Johnny" },
    { "name": "Sarah" },
    { "name": "Alice" }
  ]
}
```
```graphql
{
  allPersons {
    name
    age
    posts {
      title
    }
  }
}
```

## Queries with Arguments
```graphql
{
  allPersons(last: 2) {
    name
  }
}
```

## Writing Data with Mutations
There generally are three kinds of mutations:
- creating new data
- updating existing data
- deleting existing data

```graphql
mutation {
  createPerson(name: "Bob", age: 36) {
    name
    age
  }
}
```
```json
"createPerson": {
  "name": "Bob",
  "age": 36,
}
```

## Realtime Updates with Subscriptions
Another important requirement for many applications today is to have a realtime connection to the server in order to get immediately informed about important events.

For this use case, GraphQL offers the concept of subscriptions.

```graphql
subscription {
  newPerson {
    name
    age
  }
}
```
After a client sent this subscription to a server, a connection is opened between them. Then, whenever a new mutation is performed that creates a new `Person`, the server sends the information about this person over to the client:

```json
{
  "newPerson": {
    "name": "Jane",
    "age": 23
  }
}
```

## Defining a Schema
```graphql
type Query {
  allPersons(last: Int): [Person!]!
}

type Mutation {
  createPerson(name: String!, age: Int!): Person!
}

type Subscription {
  newPerson: Person!
}

type Person {
  name: String!
  age: Int!
  posts: [Post!]!
}

type Post {
  title: String!
  author: Person!
}
```

Big Picture (Architecture)
===

## Use Cases

**1. GraphQL server with a connected database**
![connected-database](https://imgur.com/kC0cFk7.png)

**2. GraphQL layer that integrates existing systems**
![existing-system](https://imgur.com/168FvP4.png)

**3. Hybrid approach with connected database and integration of existing system**
![hybrid-approch](https://imgur.com/oOVYriG.png)

## Resolver Functions
![resolver](https://imgur.com/cP2i8Da.png)


Reading List:
===
1. [howtographql](https://www.howtographql.com)
2. [top-5-reasons-to-use-graphql-b60cfa683511](https://www.prisma.io/blog/top-5-reasons-to-use-graphql-b60cfa683511)
