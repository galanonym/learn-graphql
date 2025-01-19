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

The query above will should return something like this:

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

The query above will should return something like this:

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

A normal:
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
