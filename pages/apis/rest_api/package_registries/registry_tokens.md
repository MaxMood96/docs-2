# Registry tokens API

The registry tokens API endpoint lets you create and manage credentials needed to install and use packages in a registry.

## Create a registry token

```bash
curl -H "Authorization: Bearer $TOKEN" \
  -X POST "https://api.buildkite.com/v2/packages/organizations/{org.slug}/registries/{registry.slug}/tokens"  \
  -H "Content-Type: application/json" \
  -d '{
    "description": "Usher"
    }'
```

```json
{
  "id": "0191b6a2-aa51-70d0-8a5f-aabce115b0fd",
  "graphql_id": "UmVnaXN0cnlUb2tlbi0tLTAxOTFiNmEyLWFhNTEtNzBkMC04YTVmLWFhYmNlMTE1YjBmZA==",
  "description": "Usher",
  "url": "http://api.buildkite.com/v2/packages/organizations/my_great_org/registries/my-registry/tokens/0191b6a2-aa51-70d0-8a5f-aabce115b0fd",
  "created_at": "2024-09-03T06:46:39.441Z",
  "created_by": {
    "id": "0191b13b-0eb6-470d-a4c0-2085974f3580",
    "graphql_id": "VXNlci0tLTAxOTFiMTNiLTBlYjYtNDcwZC1hNGMwLTIwODU5NzRmMzU4MA==",
    "name": "Eminem",
    "email": "eminem@buildkite.com",
    "avatar_url": null,
    "created_at": "2024-09-02T05:35:23.318Z"
  },
  "organization": {
    "id": "018a456f-e581-44b6-c5a4-1d8a5f7094ee",
    "slug": "my_great_org",
    "url": "https://api.buildkite.com/v2/analytics/organizations/my_great_org",
    "web_url": "https://buildkite.com/organizations/my_great_org"
  },
  "registry": {
    "id": "018f56ef-9ef4-70f0-aba2-0f4578e3d69d",
    "graphql_id": "UmVnaXN0cnktLS0wMThmNTZlZi05ZWY0LTcwZjAtYWJhMi0wZjQ1NzhlM2Q2OWQ=",
    "slug": "my-registry",
    "url": "http://api.buildkite.com/v2/packages/organizations/my_great_org/registries/my-registry",
    "web_url": "http://buildkite.com/organizations/buildkite/my_great_org/registries/my-registry"
  }
}
```

Required [request body properties](/docs/api#request-body-properties):

<table class="responsive-table">
<tbody>
  <tr><th><code>description</code></th><td>Description of the token<br><em>Example:</em> <code>"Usher"</code>.</td></tr>
</tbody>
</table>

Required scope: `write_registries`

Success response: `200 OK`

## List all registry tokens

Returns a list of a registry's tokens.

```bash
curl -H "Authorization: Bearer $TOKEN" \
  -X GET "https://api.buildkite.com/v2/packages/organizations/{org.slug}/registries/{registry.slug}/tokens" \
  -H "Content-Type: application/json"
```

```json
[
  {
    "id": "0191b6a2-aa51-70d0-8a5f-aabce115b0fd",
    "graphql_id": "UmVnaXN0cnlUb2tlbi0tLTAxOTFiNmEyLWFhNTEtNzBkMC04YTVmLWFhYmNlMTE1YjBmZA==",
    "description": "Usher",
    "url": "http://api.buildkite.com/v2/packages/organizations/my_great_org/registries/my-registry/tokens/0191b6a2-aa51-70d0-8a5f-aabce115b0fd",
    "created_at": "2024-09-03T06:46:39.441Z",
    "created_by": {
      "id": "0191b13b-0eb6-470d-a4c0-2085974f3580",
      "graphql_id": "VXNlci0tLTAxOTFiMTNiLTBlYjYtNDcwZC1hNGMwLTIwODU5NzRmMzU4MA==",
      "name": "Eminem",
      "email": "eminem@buildkite.com",
      "avatar_url": null,
      "created_at": "2024-09-02T05:35:23.318Z"
    },
    "organization": {
      "id": "018a456f-e581-44b6-c5a4-1d8a5f7094ee",
      "slug": "my_great_org",
      "url": "https://api.buildkite.com/v2/analytics/organizations/my_great_org",
      "web_url": "https://buildkite.com/organizations/my_great_org"
    },
    "registry": {
      "id": "018f56ef-9ef4-70f0-aba2-0f4578e3d69d",
      "graphql_id": "UmVnaXN0cnktLS0wMThmNTZlZi05ZWY0LTcwZjAtYWJhMi0wZjQ1NzhlM2Q2OWQ=",
      "slug": "my-registry",
      "url": "http://api.buildkite.com/v2/packages/organizations/my_great_org/registries/my-registry",
      "web_url": "http://buildkite.com/organizations/buildkite/my_great_org/registries/my-registry"
    }
  }
]
```

Required scope: `read_registries`

Success response: `200 OK`

## Get a registry token

Returns the details for a single registry token.

```bash
curl -H "Authorization: Bearer $TOKEN" \
  -X GET "https://api.buildkite.com/v2/packages/organizations/{org.slug}/registries/{registry.slug}/tokens/{id}"
```

```json
{
  "id": "0191b6a2-aa51-70d0-8a5f-aabce115b0fd",
  "graphql_id": "UmVnaXN0cnlUb2tlbi0tLTAxOTFiNmEyLWFhNTEtNzBkMC04YTVmLWFhYmNlMTE1YjBmZA==",
  "description": "Usher",
  "url": "http://api.buildkite.com/v2/packages/organizations/my_great_org/registries/my-registry/tokens/0191b6a2-aa51-70d0-8a5f-aabce115b0fd",
  "created_at": "2024-09-03T06:46:39.441Z",
  "created_by": {
    "id": "0191b13b-0eb6-470d-a4c0-2085974f3580",
    "graphql_id": "VXNlci0tLTAxOTFiMTNiLTBlYjYtNDcwZC1hNGMwLTIwODU5NzRmMzU4MA==",
    "name": "Eminem",
    "email": "eminem@buildkite.com",
    "avatar_url": null,
    "created_at": "2024-09-02T05:35:23.318Z"
  },
  "organization": {
    "id": "018a456f-e581-44b6-c5a4-1d8a5f7094ee",
    "slug": "my_great_org",
    "url": "https://api.buildkite.com/v2/analytics/organizations/my_great_org",
    "web_url": "https://buildkite.com/organizations/my_great_org"
  },
  "registry": {
    "id": "018f56ef-9ef4-70f0-aba2-0f4578e3d69d",
    "graphql_id": "UmVnaXN0cnktLS0wMThmNTZlZi05ZWY0LTcwZjAtYWJhMi0wZjQ1NzhlM2Q2OWQ=",
    "slug": "my-registry",
    "url": "http://api.buildkite.com/v2/packages/organizations/my_great_org/registries/my-registry",
    "web_url": "http://buildkite.com/organizations/buildkite/my_great_org/registries/my-registry"
  }
}
```

Required scope: `read_registries`

Success response: `200 OK`

## Update a registry token

```bash
curl -H "Authorization: Bearer $TOKEN" \
  -X POST "https://api.buildkite.com/v2/packages/organizations/{org.slug}/registries/{registry.slug}/tokens/{id}" \
  -H "Content-Type: application/json" \
  -d '{
    "description": "Usher"
    }' 
```

```json
{
  "id": "0191b6a2-aa51-70d0-8a5f-aabce115b0fd",
  "graphql_id": "UmVnaXN0cnlUb2tlbi0tLTAxOTFiNmEyLWFhNTEtNzBkMC04YTVmLWFhYmNlMTE1YjBmZA==",
  "description": "Usher",
  "url": "http://api.buildkite.com/v2/packages/organizations/my_great_org/registries/my-registry/tokens/0191b6a2-aa51-70d0-8a5f-aabce115b0fd",
  "created_at": "2024-09-03T06:46:39.441Z",
  "created_by": {
    "id": "0191b13b-0eb6-470d-a4c0-2085974f3580",
    "graphql_id": "VXNlci0tLTAxOTFiMTNiLTBlYjYtNDcwZC1hNGMwLTIwODU5NzRmMzU4MA==",
    "name": "Eminem",
    "email": "eminem@buildkite.com",
    "avatar_url": null,
    "created_at": "2024-09-02T05:35:23.318Z"
  },
  "organization": {
    "id": "018a456f-e581-44b6-c5a4-1d8a5f7094ee",
    "slug": "my_great_org",
    "url": "https://api.buildkite.com/v2/analytics/organizations/my_great_org",
    "web_url": "https://buildkite.com/organizations/my_great_org"
  },
  "registry": {
    "id": "018f56ef-9ef4-70f0-aba2-0f4578e3d69d",
    "graphql_id": "UmVnaXN0cnktLS0wMThmNTZlZi05ZWY0LTcwZjAtYWJhMi0wZjQ1NzhlM2Q2OWQ=",
    "slug": "my-registry",
    "url": "http://api.buildkite.com/v2/packages/organizations/my_great_org/registries/my-registry",
    "web_url": "http://buildkite.com/organizations/buildkite/my_great_org/registries/my-registry"
  }
}
```

Required [request body properties](/docs/api#request-body-properties):

<table class="responsive-table">
<tbody>
  <tr><th><code>description</code></th><td>Description of the token<br><em>Example:</em> <code>"Usher"</code>.</td></tr>
</tbody>
</table>

Required scope: `write_registries`

Success response: `200 OK`


## Delete a registry token

```bash
curl -H "Authorization: Bearer $TOKEN" \
  -X DELETE "https://api.buildkite.com/v2/packages/organizations/{org.slug}/registries/{registry.slug}/tokens/{id}"
```

Required scope: `write_registries`

Success response: `200 OK`
