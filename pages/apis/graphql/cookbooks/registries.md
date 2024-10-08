# Registries

A collection of common tasks with package registries using the GraphQL API.

You can test out the Buildkite GraphQL API using the [Buildkite explorer](https://graphql.buildkite.com/explorer). This includes built-in documentation under the **Docs** panel.

## List organization registries

List the first 50 registries in the organization.

```graphql
query getOrganizationRegistries {
  organization(slug: "organization-slug"){
    registries(first: 50){
      edges{
        node{
          name
          id
          uuid
          createdAt
          updateaAt
        }
      }
    }
  }
}
```
