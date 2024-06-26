---
#  _____   ____    _   _  ____ _______   ______ _____ _____ _______
#  |  __  / __   |  | |/ __ __   __| |  ____|  __ _   _|__   __|
#  | |  | | |  | | |  | | |  | | | |    | |__  | |  | || |    | |
#  | |  | | |  | | | . ` | |  | | | |    |  __| | |  | || |    | |
#  | |__| | |__| | | |  | |__| | | |    | |____| |__| || |_   | |
#  |_____/ ____/  |_| _|____/  |_|    |______|_____/_____|  |_|
#  This file is auto-generated by script/generate_graphql_api_content.sh,
#  please build the schema.graphql by running `rails graphql:update_reference_schema`
#  with https://github.com/buildkite/buildkite/,
#  replace the content in data/schema.graphql
#  and run the generation script `./scripts/generate-graphql-api-content.sh`.

title: TeamRegistry – Objects – GraphQL API
toc: false
---
<!-- vale off -->
<h1 class="has-pills">
  TeamRegistry
  <span data-algolia-exclude><span class="pill pill--object pill--normal-case pill--large"><code>OBJECT</code></span></span>
</h1>
<!-- vale on -->


A registry that's been assigned to a team

<table class="responsive-table responsive-table--single-column-rows">
  <thead>
    <th>
      <h2 data-algolia-exclude>Fields</h2>
    </th>
  </thead>
  <tbody>
    <tr><td><h3 class="is-small has-pills"><code>accessLevel</code><a href="/docs/apis/graphql/schemas/enum/registryaccesslevels" class="pill pill--enum pill--normal-case pill--medium" title="Go to ENUM RegistryAccessLevels"><code>RegistryAccessLevels!</code></a></h3><p>The access level users have to this registry</p></td></tr><tr><td><h3 class="is-small has-pills"><code>createdAt</code><a href="/docs/apis/graphql/schemas/scalar/datetime" class="pill pill--scalar pill--normal-case pill--medium" title="Go to SCALAR DateTime"><code>DateTime!</code></a></h3><p>The time when the registry was added</p></td></tr><tr><td><h3 class="is-small has-pills"><code>createdBy</code><a href="/docs/apis/graphql/schemas/object/user" class="pill pill--object pill--normal-case pill--medium" title="Go to OBJECT User"><code>User</code></a></h3><p>The user that added this registry to the team</p></td></tr><tr><td><h3 class="is-small has-pills"><code>id</code><a href="/docs/apis/graphql/schemas/scalar/id" class="pill pill--scalar pill--normal-case pill--medium" title="Go to SCALAR ID"><code>ID!</code></a></h3></td></tr><tr><td><h3 class="is-small has-pills"><code>permissions</code><a href="/docs/apis/graphql/schemas/object/teamregistrypermissions" class="pill pill--object pill--normal-case pill--medium" title="Go to OBJECT TeamRegistryPermissions"><code>TeamRegistryPermissions!</code></a></h3></td></tr><tr><td><h3 class="is-small has-pills"><code>registry</code><a href="/docs/apis/graphql/schemas/object/registry" class="pill pill--object pill--normal-case pill--medium" title="Go to OBJECT Registry"><code>Registry</code></a></h3><p>The registry associated with this team member</p></td></tr><tr><td><h3 class="is-small has-pills"><code>team</code><a href="/docs/apis/graphql/schemas/object/team" class="pill pill--object pill--normal-case pill--medium" title="Go to OBJECT Team"><code>Team</code></a></h3><p>The team associated with this team member</p></td></tr><tr><td><h3 class="is-small has-pills"><code>updatedAt</code><a href="/docs/apis/graphql/schemas/scalar/datetime" class="pill pill--scalar pill--normal-case pill--medium" title="Go to SCALAR DateTime"><code>DateTime!</code></a></h3><p>The time when the assignment was last updated</p></td></tr><tr><td><h3 class="is-small has-pills"><code>updatedBy</code><a href="/docs/apis/graphql/schemas/object/user" class="pill pill--object pill--normal-case pill--medium" title="Go to OBJECT User"><code>User</code></a></h3><p>The user that last updated this assignment</p></td></tr><tr><td><h3 class="is-small has-pills"><code>uuid</code><a href="/docs/apis/graphql/schemas/scalar/string" class="pill pill--scalar pill--normal-case pill--medium" title="Go to SCALAR String"><code>String!</code></a></h3><p>The public UUID for this team registry</p></td></tr>
  </tbody>
</table>




<h2 data-algolia-exclude>Interfaces</h2>
<div>
  <a href="/docs/apis/graphql/schemas/interface/node" class="pill pill--interface pill--normal-case pill--large" title="Go to INTERFACE Node">
  <code>Node</code>
</a>

</div>
