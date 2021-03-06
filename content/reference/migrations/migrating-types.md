---
alias: xohs5xooph
path: /docs/reference/migrations/types
layout: REFERENCE
description: There are many possible options to update a GraphQL schema, referred to as schema migrations. Some of them require data migrations as well.
tags:
  - migrations
related:
  further:
  more:
---

# Migrating Types

GraphQL types in your GraphQL schema can be controlled using the `type` keyword.

> Read more about GraphQL [types in the schema chapter]()

## Adding a new type

You can add new types to your GraphQL schema by adding a new `type` section in the schema file. This will automatically add [queries](), [mutations]() and [subscriptions]() to your GraphQL API.

Consider this schema file:

```graphql
type User implements Node {
  id: ID!
  name: String!
}
```

To add a new `Story` type, this is the new schema file:

```graphql
type User implements Node {
  id: ID!
  name: String!
  stories: [Story!]! @relation(name: "UserStories")
}

type Story implements Node {
  id: ID!
  isPublished: Boolean!
  text: String!
  slug: String! @isUnique
  tags: [String!]
  user: User! @relation(name: "UserStories")
}
```

This will automatically add the [system fields]() to the type as well, you don't need to specify them yourself. Using the [`@relation` directive](), you can also directly create new relations to existing types as well.

Have a look at the [naming conventions for types]() to see what names are allowed and recommended.

## Removing an existing type

To remove an existing type **including all of its data and relations**, remove the corresponding section in the schema file. You also need to remove all relation tags for relations that include the type to be deleted as well.

> Removing a type potentially breaks existing queries, mutations and subscriptions. Make sure to not rely on any operations on the type in your apps before deleting it.

Consider this schema file:

```graphql
type User implements Node {
  id: ID!
  name: String!
  stories: [Story!]! @relation(name: "UserStories")
}

type Story implements Node {
  id: ID!
  isPublished: Boolean!
  text: String!
  slug: String! @isUnique
  tags: [String!]
  user: User! @relation(name: "UserStories")
}
```

To remove the `Story` type again, we need to remove the `type Story` section as well as the relation field `stories` on `User`:

```graphql
type User implements Node {
  id: ID!
  name: String!
}
```

## Renaming an existing type

Renaming a type can be done with the `@rename(oldName: String!)` directive.

> Renaming a type potentially breaks existing queries, mutations and subscriptions. Make sure to adjust your app accordingly to the new name.

Consider this schema file:

```graphql
type User implements Node {
  id: ID!
  name: String!
  stories: [Story!]! @relation(name: "UserStories")
}

type Story implements Node {
  id: ID!
  isPublished: Boolean!
  text: String!
  slug: String! @isUnique
  tags: [String!]
  user: User! @relation(name: "UserStories")
}
```

To rename the `Story` type, we use the [temporary directive]() `@rename(oldName: String!)` on the type itself. We also need to update the type name for all relation fields that use the old type name. In this case, that's the `stories: [Story!]!` field.

This is how it looks like:


```graphql
type User implements Node {
  id: ID!
  name: String!
  stories: [Post!]! @relation(name: "UserStories")
}

type Post implements Node @rename(oldName: "Story") {
  id: ID!
  isPublished: Boolean!
  text: String!
  slug: String! @isUnique
  tags: [String!]
  user: User! @relation(name: "UserStories")
}
```

After the successful rename operation, we obtain this new schema file:

```graphql
type User implements Node {
  id: ID!
  name: String!
  stories: [Post!]! @relation(name: "UserStories")
}

type Post implements Node {
  id: ID!
  isPublished: Boolean!
  text: String!
  slug: String! @isUnique
  tags: [String!]
  user: User! @relation(name: "UserStories")
}
```

Note that the temporary directive `@rename` is not in the schema file anymore.
