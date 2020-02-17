# Using unions

```javascript
const DATA = [
  { username : 'catherine' },
  { director : 'catherine hardwicke' },
  { author : 'catherine cookson' }
];

const UserType = new GraphQLObjectType({
  name : 'User',
  fields : {
    username : { type : GraphQLString }
  }
});

const MovieType = new GraphQLObjectType({
  name : 'Movie',
  fields : {
    director : { type : GraphQLString }
  }
});

const BookType = new GraphQLObjectType({
  name : 'Book',
  fields : {
    author : { type : GraphQLString }
  }
});

const resolveType = (data) => {
  if (data.username) {
    return UserType;
  }
  if (data.director) {
    return MovieType;
  }
  if (data.author) {
    return BookType;
  }
};

const SearchableType = new GraphQLUnionType({
  name: 'SearchableType',
  types: [ UserType, MovieType, BookType ],
  resolveType: resolveType
});

const schema = new GraphQLSchema({
  query: new GraphQLObjectType({
    name: 'RootQueryType',
    fields: {
      search: {
        type: new GraphQLList(SearchableType),
        args:  {
          text: { type : new GraphQLNonNull(GraphQLString) }
        },
        resolve(root, args) {
          const text = args.text;
          return DATA.filter((d) => {
            const searchableProperty = d.username || d.director || d.author;
            return searchableProperty.indexOf(text) !== -1;
          });
        }
      }
    }
  })
});

const query = `
  {
    search(text: "cat") {
      ... on User {
        username
      }
      ... on Movie {
        director
      }
      ... on Book {
        author
      }
    }
  }
`;
```

# Using interfaces

```javascript
const DATA = [
  { username : 'catherine' },
  { director : 'catherine hardwicke' },
  { author : 'catherine cookson' }
];

const UserType = new GraphQLObjectType({
  name : 'User',
  interfaces: [SearchableType],
  fields : {
    username : {
      type : GraphQLString
    },
    searchPreviewText : {
      type : GraphQLString,
      resolve(data) {
        return `(user) ${data.username}`;
      }
    }
  }
  // isTypeOf: data => !!data.username;
});

const MovieType = new GraphQLObjectType({
  name : 'Movie',
  fields : {
    director : { type : GraphQLString }
  }
});

const BookType = new GraphQLObjectType({
  name : 'Book',
  fields : {
    author : { type : GraphQLString }
  }
});

// can also add isTypeOf on every type instead of resolve type
const resolveType = (data) => {
  if (data.username) {
    return UserType;
  }
  if (data.director) {
    return MovieType;
  }
  if (data.author) {
    return BookType;
  }
};

const SearchableType = new GraphQLInterfaceType({
  name: 'Searchable',
  fields: {
    searchPreviewText: { type: GraphQLString }
  },
  resolveType: resolveType
});

const schema = new GraphQLSchema({
  types: [MovieType, BookType, UserType, SearchableType],
  query: new GraphQLObjectType({
    name: 'RootQueryType',
    fields: {
      search: {
        type: new GraphQLList(SearchableType),
        args:  {
          text: { type : new GraphQLNonNull(GraphQLString) }
        },
        resolve(root, args) {
          const text = args.text;
          return DATA.filter((d) => {
            const searchableProperty = d.username || d.director || d.author;
            return searchableProperty.indexOf(text) !== -1;
          });
        }
      }
    }
  })
});

const query = `
  {
    search(text: "cat") {
      ... on User {
        username
      }
      ... on Movie {
        director
      }
      ... on Book {
        author
      }
    }
  }
`;
```



# Resources

-  [[Article] [Medium] GraphQL Tour: Interfaces and Unions](https://medium.com/the-graphqlhub/graphql-tour-interfaces-and-unions-7dd5be35de0d)

