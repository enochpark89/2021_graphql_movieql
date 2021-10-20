# Make movieAPI with GraphQL

-  GraphQL makes the backend developmenet super easy.
   -  apollo and facebook uses graphQL.

# development progress

1. Set up working environment.
2. Install graphQL

```
npm add graphql-yoga
```
# 1.0 Problems Solved by GraphQL

- Over-fetching and Under-fetching.

*What is over-fetching?*
```
/users/ GET 
// this might get many information about users such as names
// After that based on the information you need, you have to change the URL such as /users/age etc.
```
- If users get more information from the user, this is called over-fetching. 
   - We can ask Front-end to ask DB for only the username. 

- 

*What is under-fetching?*
- you have to make a lot of request to accomplish only one thing. 
- For example, if you want to get
/feed/
/notifications/
/user/1/
- you need to have 3 requests to get the information needed. 
- With GraphQL, you can fetch all three items with one URL. 
- On GraphQL, there is only one endpoint. There is no URL. 
**Query: Ask something from the DB.**
- Example of GraphQL query:
```js
{
   feed{
      comments
      likeNumber
   }
   notifications {
      isRead
   }
   user {
      username
      profilePic
   }
}
// This query can be sent to get answers
```
- Output:
```js
{
   feed: [
      {
         comments:1,
         likeNumber: 20
      }
   ],
   notifications: [
      {
         isRead:true
      },
      {
         isRead: false
      }
   ],
   user: {
      username: "nico"
      profile: "http:"
   }
}
```

# 2.0 Creating a GraphQL Server with GraphQL Yoga

- Create a script called nodemon
- Nodemon is going to be looking for the main index.js file. 

*What is resolver?*
- Goto the documentation and follow the quickstart of GraphQL Yoga.
- If you DO NOT want to use old code, you have to install babel node. 
```
npm add babel-node --dev
// also npm add babel-cli
```

- Also install by using below code:
```
npm -g add babel-cli babel-preset-env babel-preset-stage-3
```

*How do you start a server?*

index.js
```js
import { GraphQLServer } from "graphql-yoga";

const server = new GraphQLServer({});

server.start(() => console.log("Graphql Server Running"));
```

- you can't start a server (with error)because you do not have a schema.

*What is schema?*
- Schema is a description of data that you are going to GIVE to the user or GET from the user.

# Creating the first Query and Resolver

- **Schema:** describe what are you going to get or send. 
- Create a new folder called graphql and create schema.graphql.
- Get data from the DB - Query
- Push data to the DB - Mutation
- Mutation - when you change the data within your server. 

- We have to specify which mutation and which query that we want. 

*What is resolver?*
- We are describing what happen when the user send a query.
- NodeJS has to do something. We need to program the functionality of name. 
- Resolver resolves the query. Query is a question and we need to resolve it in some ways. 

*How to do you set up resolver and schema?*

graphql/schema.graphql
```js
// when there is a query, we are going to send or receive required string.
type Query {
  name: String!
}
```

graphql/resolver
```js
// The query is going to return the string "nicolas"
const resolvers = {
    Query: {
      name: () => "nicolas"
    }
  };
  
  export default resolvers;
```

- If you include both schema and resolvers, the server will start running. 
index.js
```js
import { GraphQLServer } from "graphql-yoga";
import resolvers from "./graphql/resolver"

const server = new GraphQLServer({
  typeDefs: "graphql/schema.graphql",
  resolvers
});

server.start(() => console.log("Graphql Server Running"));

```

Output:
Graphql Server Running

- If you go to localhost:4000, you will see the GraphQL playground.