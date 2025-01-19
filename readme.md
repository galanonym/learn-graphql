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

`query` - root operation type, can be *query*, *mutation* or *subscription*

`HeroNameAndFriends` - optional *operation name* for debugging and tracing

`hero`, `name`, `friends`, `name` - section is called *selection set*

`hero`, `name`, `friends`, `name` - are all called *fields*

The query returns json array with `data` top level key

Or returns `errors` top level key

GraphQL queries look the same for single items or lists of items; expected based on schema

### The query above will should return something like this:

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

The arguments are in (). What arguments can be used and how are defined by schema of the API.
