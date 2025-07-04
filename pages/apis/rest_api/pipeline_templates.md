# Pipeline templates API

> 📘 Enterprise feature
> [Pipeline templates](/docs/pipelines/governance/templates) are only available on an [Enterprise](https://buildkite.com/pricing) plan.

The pipeline templates API endpoint allows admins to create and manage pipeline templates for an organization.
Non-admins can only read or assign pipeline templates marked as `available` by organization admins.

## Pipeline template data model

<table class="responsive-table">
<tbody>
  <tr><th><code>uuid</code></th><td>UUID of the pipeline template</td></tr>
  <tr><th><code>graphql_id</code></th><td><a href="/docs/apis/graphql-api#graphql-ids">GraphQL ID of the pipeline template</a></td></tr>
  <tr><th><code>name</code></th><td>Name of the pipeline template</td></tr>
  <tr><th><code>description</code></th><td>Description of the pipeline template</td></tr>
  <tr><th><code>configuration</code></th><td>YAML step configuration for the pipeline template</td></tr>
  <tr><th><code>available</code></th><td>When set to <code>true</code>, non-admins can assign the pipeline template to pipelines<br><em>Default:</em> <code>false</code></td></tr>
  <tr><th><code>url</code></th><td>Canonical API URL of the pipeline template</td></tr>
  <tr><th><code>web_url</code></th><td>URL of the pipeline template on Buildkite</td></tr>
  <tr><th><code>created_at</code></th><td>When the pipeline template was created</td></tr>
  <tr><th><code>created_by</code></th><td><a href="/docs/apis/rest-api/user">User</a> who created the pipeline template</td></tr>
  <tr><th><code>updated_at</code></th><td>When the pipeline template was created</td></tr>
  <tr><th><code>updated_by</code></th><td><a href="/docs/apis/rest-api/user">User</a> who last updated the pipeline template</td></tr>
</tbody>
</table>

## List pipeline templates

Returns a list of an organization's pipeline templates.

```bash
curl -H "Authorization: Bearer $TOKEN" \
  -X GET "https://api.buildkite.com/v2/organizations/{org.slug}/pipeline-templates"
```

```json
[
  {
    "uuid": "018a86cc-db73-7d15-8c68-5023cf8d64c3",
    "graphql_id": "UGlwZWxpbmVUZW1wbGF0ZS0tLTAxOGE4NmNjLWRiNzMtN2QxNS04YzY4LTUwMjNjZjhkNjRjMw==",
    "name": "Build template",
    "description": "Shared build steps configuration",
    "configuration": "steps:\n  - label: \":hammer: Build\"\n    command: \"scripts/build.sh\"",
    "available": false,
    "url": "http:///api.buildkite.com/v2/organizations/acme-inc/pipeline-templates/018a86cc-db73-7d15-8c68-5023cf8d64c3",
    "web_url": "http://www.buildkite.com/organizations/acme-inc/pipeline-templates/018a86cc-db73-7d15-8c68-5023cf8d64c3",
    "created_at": "2023-05-03T04:17:55.867Z",
    "created_by": {
      "id": "3d3c3bf0-7d58-4afe-8fe7-b3017d5504de",
      "graphql_id": "VXNlci0tLTNkM2MzYmYwLTdkNTgtNGFmZS04ZmU3LWIzMDE3ZDU1MDRkZQo=",
      "name": "Sam Kim",
      "email": "sam@example.com",
      "avatar_url": "https://www.gravatar.com/avatar/example",
      "created_at": "2013-05-03T04:17:55.867Z"
    },
    "updated_at": "2023-06-12T04:17:55.867Z",
    "updated_by": {
      "id": "3d3c3bf0-7d58-4afe-8fe7-b3017d5504de",
      "graphql_id": "VXNlci0tLTNkM2MzYmYwLTdkNTgtNGFmZS04ZmU3LWIzMDE3ZDU1MDRkZQo=",
      "name": "Sam Kim",
      "email": "sam@example.com",
      "avatar_url": "https://www.gravatar.com/avatar/example",
      "created_at": "2013-05-03T04:17:55.867Z"
    }
  }
]
```

Required scope: `read_pipeline_templates`

Success response: `200 OK`

## Get a pipeline template

```bash
curl -H "Authorization: Bearer $TOKEN" \
  -X GET "https://api.buildkite.com/v2/organizations/{org.slug}/pipeline-templates/{uuid}"
```

```json
{
  "uuid": "018a86cc-db73-7d15-8c68-5023cf8d64c3",
  "graphql_id": "UGlwZWxpbmVUZW1wbGF0ZS0tLTAxOGE4NmNjLWRiNzMtN2QxNS04YzY4LTUwMjNjZjhkNjRjMw==",
  "name": "Build template",
  "description": "Shared build steps configuration",
  "configuration": "steps:\n  - label: \":hammer: Build\"\n    command: \"scripts/build.sh\"",
  "available": false,
  "url": "http:///api.buildkite.com/v2/organizations/acme-inc/pipeline-templates/018a86cc-db73-7d15-8c68-5023cf8d64c3",
  "web_url": "http://www.buildkite.com/organizations/acme-inc/pipeline-templates/018a86cc-db73-7d15-8c68-5023cf8d64c3",
  "created_at": "2023-05-03T04:17:55.867Z",
  "created_by": {
    "id": "3d3c3bf0-7d58-4afe-8fe7-b3017d5504de",
    "graphql_id": "VXNlci0tLTNkM2MzYmYwLTdkNTgtNGFmZS04ZmU3LWIzMDE3ZDU1MDRkZQo=",
    "name": "Sam Kim",
    "email": "sam@example.com",
    "avatar_url": "https://www.gravatar.com/avatar/example",
    "created_at": "2023-08-29T10:10:03.000Z"
  },
   "updated_at": "2023-06-12T04:17:55.867Z",
  "updated_by": {
    "id": "3d3c3bf0-7d58-4afe-8fe7-b3017d5504de",
    "graphql_id": "VXNlci0tLTNkM2MzYmYwLTdkNTgtNGFmZS04ZmU3LWIzMDE3ZDU1MDRkZQo=",
    "name": "Sam Kim",
    "email": "sam@example.com",
    "avatar_url": "https://www.gravatar.com/avatar/example",
    "created_at": "2013-05-03T04:17:55.867Z"
  }
}
```

Required scope: `read_pipeline_templates`

Success response: `200 OK`

## Create a pipeline template

```bash
curl -H "Authorization: Bearer $TOKEN" \
  -X POST "https://api.buildkite.com/v2/organizations/{org.slug}/pipeline-templates" \
  -H "Content-Type: application/json" \
  -d '{
    "name": ":hammer: Build",
    "description": "Shared build steps configuration",
    "configuration": "steps:\n  - label: \":hammer: Build\"\n    command: \"scripts/build.sh\"",
    "available": true
  }'
```

```json
{
  "uuid": "018a86cc-db73-7d15-8c68-5023cf8d64c3",
  "graphql_id": "UGlwZWxpbmVUZW1wbGF0ZS0tLTAxOGE4NmNjLWRiNzMtN2QxNS04YzY4LTUwMjNjZjhkNjRjMw==",
  "name": "Build template",
  "description": "Shared build steps configuration",
  "configuration": "steps:\n  - label: \":hammer: Build\"\n    command: \"scripts/build.sh\"",
  "available": true,
  "url": "http:///api.buildkite.com/v2/organizations/acme-inc/pipeline-templates/018a86cc-db73-7d15-8c68-5023cf8d64c3",
  "web_url": "http://www.buildkite.com/organizations/acme-inc/pipeline-templates/018a86cc-db73-7d15-8c68-5023cf8d64c3",
  "created_at": "2023-05-03T04:17:55.867Z",
  "created_by": {
    "id": "3d3c3bf0-7d58-4afe-8fe7-b3017d5504de",
    "graphql_id": "VXNlci0tLTNkM2MzYmYwLTdkNTgtNGFmZS04ZmU3LWIzMDE3ZDU1MDRkZQo=",
    "name": "Sam Kim",
    "email": "sam@example.com",
    "avatar_url": "https://www.gravatar.com/avatar/example",
    "created_at": "2023-08-29T10:10:03.000Z"
  },
  "updated_at": "2023-06-12T04:17:55.867Z",
  "updated_by": {
    "id": "3d3c3bf0-7d58-4afe-8fe7-b3017d5504de",
    "graphql_id": "VXNlci0tLTNkM2MzYmYwLTdkNTgtNGFmZS04ZmU3LWIzMDE3ZDU1MDRkZQo=",
    "name": "Sam Kim",
    "email": "sam@example.com",
    "avatar_url": "https://www.gravatar.com/avatar/example",
    "created_at": "2013-05-03T04:17:55.867Z"
  }
}
```

Required [request body properties](/docs/api#request-body-properties):

<table class="responsive-table">
<tbody>
  <tr>
    <th><code>name</code></th>
    <td>Name for the pipeline template.<br><em>Example:</em> <code>"Build template"</code></td>
  </tr>
  <tr>
    <th><code>configuration</code></th>
    <td>YAML step configuration for the pipeline template.<br><em>Example:</em> <code>"steps:\n  - command: "scripts/build.sh"</code></td>
  </tr>
</tbody>
</table>

Optional [request body properties](/docs/api#request-body-properties):

<table class="responsive-table">
<tbody>
  <tr>
    <th><code>description</code></th>
    <td>Description for the pipeline template.<br><em>Example:</em> <code>"Shared build steps configuration"</code></td>
  </tr>
  <tr>
    <th><code>available</code></th>
    <td>When set to <code>true</code>, non-admins can assign the pipeline template to pipelines.<br><em>Example:</em> <code>false</code></td>
  </tr>
</tbody>
</table>

Required scope: `write_pipeline_templates`

Success response: `201 Created`

Error responses:

<table class="responsive-table">
<tbody>
  <tr><th><code>422 Unprocessable Entity</code></th><td><code>{ "message": "Validation failed: Reason for failure" }</code></td></tr>
</tbody>
</table>

## Update a pipeline template

```bash
curl -H "Authorization: Bearer $TOKEN" \
  -X PATCH "https://api.buildkite.com/v2/organizations/{org.slug}/pipeline-templates/{uuid}" \
  -H "Content-Type: application/json" \
  -d '{ "available": true }'
```

```json
{
  "uuid": "018a86cc-db73-7d15-8c68-5023cf8d64c3",
  "graphql_id": "UGlwZWxpbmVUZW1wbGF0ZS0tLTAxOGE4NmNjLWRiNzMtN2QxNS04YzY4LTUwMjNjZjhkNjRjMw==",
  "name": "Build template",
  "description": "Shared build steps configuration",
  "configuration": "steps:\n  - label: \":hammer: Build\"\n    command: \"scripts/build.sh\"",
  "available": true,
  "url": "http:///api.buildkite.com/v2/organizations/acme-inc/pipeline-templates/018a86cc-db73-7d15-8c68-5023cf8d64c3",
  "web_url": "http://www.buildkite.com/organizations/acme-inc/pipeline-templates/018a86cc-db73-7d15-8c68-5023cf8d64c3",
  "created_at": "2023-05-03T04:17:55.867Z",
  "created_by": {
    "id": "3d3c3bf0-7d58-4afe-8fe7-b3017d5504de",
    "graphql_id": "VXNlci0tLTNkM2MzYmYwLTdkNTgtNGFmZS04ZmU3LWIzMDE3ZDU1MDRkZQo=",
    "name": "Sam Kim",
    "email": "sam@example.com",
    "avatar_url": "https://www.gravatar.com/avatar/example",
    "created_at": "2023-08-29T10:10:03.000Z"
  },
  "updated_at": "2023-06-12T04:17:55.867Z",
  "updated_by": {
    "id": "3d3c3bf0-7d58-4afe-8fe7-b3017d5504de",
    "graphql_id": "VXNlci0tLTNkM2MzYmYwLTdkNTgtNGFmZS04ZmU3LWIzMDE3ZDU1MDRkZQo=",
    "name": "Sam Kim",
    "email": "sam@example.com",
    "avatar_url": "https://www.gravatar.com/avatar/example",
    "created_at": "2013-05-03T04:17:55.867Z"
  }
}
```

Optional [request body properties](/docs/api#request-body-properties):

<table class="responsive-table">
<tbody>
  <tr>
    <th><code>name</code></th>
    <td>Name for the pipeline template.<br><em>Example:</em> <code>"Build template"</code></td>
  </tr>
  <tr>
    <th><code>description</code></th>
    <td>Description for the pipeline template.<br><em>Example:</em> <code>"Shared build steps configuration"</code></td>
  </tr>
  <tr>
    <th><code>configuration</code></th>
    <td>YAML step configuration for the pipeline template.<br><em>Example:</em> <code>"steps:\n  - command: "scripts/build.sh"</code></td>
  </tr>
  <tr>
    <th><code>available</code></th>
    <td>When set to <code>true</code>, non-admins can assign the pipeline template to pipelines.<br><em>Example:</em> <code>false</code></td>
  </tr>
</tbody>
</table>

Required scope: `write_pipeline_templates`

Success response: `200 OK`

Error responses:

<table class="responsive-table">
<tbody>
  <tr><th><code>422 Unprocessable Entity</code></th><td><code>{ "message": "Validation failed: Reason for failure" }</code></td></tr>
</tbody>
</table>

## Delete a pipeline template

>📘
> A pipeline template can only be deleted when it is not assigned to any pipelines. Ensure you remove the pipeline template from all pipelines before trying to delete it.

```bash
curl -H "Authorization: Bearer $TOKEN" \
  -X DELETE "https://api.buildkite.com/v2/organizations/{org.slug}/pipeline-templates/{uuid}"
```

Required scope: `write_pipeline_templates`

Success response: `204 No Content`

Error responses:

<table class="responsive-table">
<tbody>
  <tr><th><code>422 Unprocessable Entity</code></th><td><code>{ "message": "Reason the pipeline template couldn't be deleted" }</code></td></tr>
</tbody>
</table>
