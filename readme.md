# GraphQL Cheatsheet

## Normal Queries

```
query HeroNameAndFriends {
  hero {
    name
    friends {
      name
    }
  }
}
```

Naming conventions:
* `query` - root operation type, can be **query**, **mutation** or **subscription**
* `HeroNameAndFriends` - optional **operation name** for debugging and tracing, but required for **mutation** and **subscription**
* `hero` `name` `friends` `name` - section is called **selection set**
* `hero` `name` `friends` `name` - are all called **fields**

Basics:
* The query returns json array with `data` top level key
* Or returns `errors` top level key
* GraphQL queries look the same for single items or lists of items; expected based on schema

The query above will return:

```
{
  "data": {
    "hero": {
      "name": "R2-D2",
      "friends": [
        {
          "name": "Luke Skywalker"
        },
        {
          "name": "Han Solo"
        },
        {
          "name": "Leia Organa"
        }
      ]
    }
  }
}
```

## With arguments

```
query HumanWithArgs {
  {
    human(id: "1000") {
      name
      height(unit: FOOT)
    }
  }
}
```

* The arguments are in (). What arguments can be used and how are defined by schema of the API.
* Arguments can be of different types: `Int` `String` `Boolean` `ENUM` (and more)
* `(unit: FOOT)` is an example of ENUM usage

The query above will return:

```
{
  "data": {
    "human": {
      "name": "Luke Skywalker",
      "height": 5.6430448
    }
  }
}
```

## Aliases

A normal query:
```
query GetUser4 {
  {
    user(id: 4) {
      id
      name
    }
  }
}
```

Will return:
```
{
  "data": {
    "user": {
      "id": 4,
      "name": "Mark Zuckerberg",
    }
  }
}
```

But you can optionally use an alias:
```
query GetUser4WithAlias {
  {
    zuck: user(id: 4) {
      id
      name
    }
  }
}

```

And it will return:
```
{
  "data": {
    "zuck": {
      "id": 4,
      "name": "Mark Zuckerberg",
    }
  }
}
```

This can be nessesary if you want to query for two specific users in the same query like this:
```
query GetUser4 {
  {
    zuck: user(id: 4) {
      id
      name
    }
    someOtherGuy: user(id: 1) {
      id
      name
    }
  }
}
```

# Variables

* GraphQL does not want you to pass dynamic variables into query with string concatenation
* Variables are passed as a seperate dictionar
* You should never do string interpolation for query part

```
#Operation
query HeroNameAndFriends($episode: Episode) {
  hero(episode: $episode) {
    name
    friends {
      name
    }
  }
}

#Variables
{
  "episode": "JEDI"
}
```

Naming conventions:
* `($episode: Episode)` - **variable definition** is declaration of the variable and its type and is after **operation name**
* `$episode` - inside the query is where the variable replaces static value
* `episode` - inside variables the key should be the same as the name of declared variable

#Inline fragments

```
query ThemeFilesPaginated($themeId: ID!) {
  theme(id: $themeId) {
    files(first: 50) {
      edges {
        node {
          body {
            ... on OnlineStoreThemeFileBodyBase64 {
              contentBase64
            }
            ... on OnlineStoreThemeFileBodyText {
              content
            }
            ... on OnlineStoreThemeFileBodyUrl {
              url
            }
          }
        }
      }
    }
  }
}
```
* The `... on` is called **inline fragment**. Fragment in GraphQL means some chosen fields of an object. Inline fragment means that we choose some fields of an object directly inline
* The `... on` syntax in GraphQL is part of an **inline fragment** that specifies **type condition**. It allows querying fields on specific types within a **union type** (or **interface type**).
* `body` is a union type - field that can directly resolve to three unrelated types, where each can have different fields (specified by schema)
* So when looping through nodes with differend body, when body type is **OnlineStoreThemeFileBodyText** it will take field `content` from that node.
* But if body type is **OnlineStoreThemeFileBodyUrl** it will take field `url` from that node.
