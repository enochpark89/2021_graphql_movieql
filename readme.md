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

# 3.0. Creating the first Query and Resolver

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

# 4.0. Extending the Schema 

- Playground is where you can test yoru code in GraphQL. 
- The root is the palyground. 
- One advantage of GraphQL is that you can 

schema.graphql
```js
type Nicolas {
  name: String!
  age: Int!
  gender: String!
}

type Query {
  person: Nicolas!
}
// GraphQL takes in type Nicolas which is a person who takes string name, Integer age, and gender string.

```


resolver.js
```js
const nicolas = {
  name: "Nicolas",
  age: 18,
  gender: "female"
};
// resolver will take the user's query into nicolas const with many different information.
const resolvers = {
  Query: {
    person: () => nicolas
  }
};
export default resolvers;

```

Playground:

Input:
```js
# Try to write your query here
query {
  person {
    age
  }
}
```
Output:
```js
{
  "data": {
    "person": {
      "age": 18
    }
  }
}
```

- More complex query.
   - Query - we are here.
   - Mutation
   - Subscription

scehma.graphql
```js
type Person {
  id: Int!
  name: String!
  age: Int!
  gender: String!
}

type Query {
  people: [Person]!
  person(id: Int!): Person
}
// Send many person.
// Maybe we won't find many persons in the resolver.
// Query will take people or a person with an ID.
```

resolver.js
```js
import { people } from "./db";

const resolvers = {
  Query: {
    people: () => people
  }
};
// resolvers will route to people in .db.js
export default resolvers;
```

db.js
```js
// people contains many users.
export const people = [
    {
      id: "0",
      name: "Nicolas",
      age: 18,
      gender: "female"
    },
    {
      id: "1",
      name: "Jisu",
      age: 18,
      gender: "female"
    },
    {
      id: "2",
      name: "Japan Guy",
      age: 18,
      gender: "male"
    },
    {
      id: "3",
      name: "Daal",
      age: 18,
      gender: "male"
    },
    {
      id: "4",
      name: "JD",
      age: 18,
      gender: "male"
    },
    {
      id: "5",
      name: "moondaddi",
      age: 18,
      gender: "male"
    },
    {
      id: "6",
      name: "flynn",
      age: 18,
      gender: "male"
    }
  ];
  
  export const getById = id => {
    const filteredPeople = people.filter(person => people.id === id);
    return filteredPeople[0];
  };
```

Playground:

input: 
```js
// Try to write your query here
query {
  people {
    name
  }
}

```
output:
```js
{
  "data": {
    "people": [
      {
        "name": "Nicolas"
      },
      {
        "name": "Jisu"
      },
      {
        "name": "Japan Guy"
      },
      {
        "name": "Daal"
      },
      {
        "name": "JD"
      },
      {
        "name": "moondaddi"
      },
      {
        "name": "flynn"
      }
    ]
  }
}
```

# 5.0. Create Queries with arguments

- Make the reesolver query by ID.

resolver.js
```js
// Make sure db.js getById function is imported
import { people, getById } from "./db";

const resolvers = {
  Query: {
    people: () => people,
    // Argument: first is an object but not important now. Second is an ID.
    person: (_, { id }) => getById(id)
  }
};

export default resolvers;
```


```js
  export const getById = id => {
    const filteredPeople = people.filter(person => person.id === String(id));
    return filteredPeople[0];
  };
```

Playground test

input:
```
{
	person(id:1) {
    name
  }
}
```

output:
```
{
  "data": {
    "person": {
      "name": "Jisu"
    }
  }
}
```

- Remember it is all about
   1. Describe what is going to happen
   2. Make it happen. 

*If you put it like a Django, resolver is like view Schema is like URL.*

# 6.0 Defining Mutations

schema.graphql
```js
type Movie {
  id: Int!
  name: String!
  score: Int!
}
type Query {
  movies: [Movie]!
  movie(id: Int!): Movie
}

type Mutation {
  addMovie(score: Int!, name: String!): Movie!
}
// If you wan tto create a Mutation, you have to call it after the query.

```

- Any db can be hooked on the backend.

# 7.0 Create First Mutation

schema.graphql:
```js
type Mutation {
  addMovie(name: String!, score: Int!): Movie!
}
// argument: name and score, return: movie.
```

resolver:
```js
import { getMovies, getById, addMovie } from "./db";

const resolvers = {
  Query: {
    movies: () => getMovies(),
    movie: (_, { id }) => getById(id)
  },
  Mutation: {
    addMovie: (_, { name, score }) => addMovie(name, score)
  }
};

export default resolvers;
```

db.js:
```js
// addMovie() function will add a Movie that consist of name and score. Id gets automatically generated by default.
export const addMovie = (name, score) => {
  const newMovie = {
    id: `${movies.length + 1}`,
    name,
    score
  };
  movies.push(newMovie);
  return newMovie;
};
```