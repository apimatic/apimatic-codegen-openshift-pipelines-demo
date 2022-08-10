While generating an API Portal, you have the ability to filter out Endpoints and Models based on Roles. This can be done by providing the Role ID(s) in the Build file as an array of strings.

To use this feature, you are be required to:

* Update the API Specification file to contain Roles information. See the section *Roles in API Specification* below.
* Mark the Endpoints in the API Specification file with Tags. Learn more in section *Mark Endpoints with Tags* below.
* Update the `APIMATIC-BUILD.json` file to provide Role ID(s) as shown below

```json
{
  "$schema": "https://titan.apimatic.io/api/build/schema",
  "buildFileVersion": "1",
  "generatePortal": {
    "languageConfig": {
      "http": {}
    },
    "filterByRole": ["pub"]
  }
}
```

If any of the provided Role IDs do not exist in the API Specification file, the Portal Generation will fail with following error:

```json
{
    "error": "Could not filter API against the provided roles. Invalid Role"
}
```

*Note that `filterByRole` is not mandatory. You can choose to leave it out of the `APIMATIC-BUILD.json` file, or provide an empty array, if you do not wish to perform any filter your API endpoints.*


If you choose to filter your API by roles, the resulting Portal will only contain documentation for those Endpoints tagged with the provided Roles.

### Roles in API Specification

API Filtration works by matching the Tags of the provided Roles with the Tags of the Endpoints. If filtration is applied while generating the Portal, only those Endpoints will be documented that have at least one Tag from the Tags of the provided Roles.

Roles information can be provided in the API Specification file for different formats as described below:

#### OpenAPI (`v2.0`, `v3.0`)

Information regarding roles can be added in OpenAPI description files using the `x-roles` extension as shown below:

```yaml
info:
  title: Swagger Petstore
x-roles:
  - name: Public Endpoints
    id: pub
    tags: 
      - pets
    description: This role only allows endpoints that should be visible to the public.
```

More details about this extension and its usage can be found [here](https://docs.apimatic.io/advanced/swagger-codegen-extensions/#api-filtering-roles-extension).

#### RAML (`v1.0`)

Information regarding roles can be added in RAML `v1.0` files using the `(roles)` annotation as shown below:

```yaml
(roles):
- name: Public Endpoints
  id: pub
  description: This role only allows endpoints that should be visible to the public.
  tags:
  - pets
```

More details about this annotation and its usage can be found [here](https://docs.apimatic.io/advanced/raml-apimatic-annotations/#role-based-access-annotations).


### Mark Endpoints with Tags

Endpoints need to be Tagged in order for API filtration to work. If API filtration is enabled, Endpoints with no Tags will not be documented in the generated Portal.

Endpoints can be tagged in the API Specification file for different formats as described below:

#### OpenAPI (`v2.0`, `v3.0`)

Use OpenAPI's operation level list of `tags` for this purpose, as shown below:

```yaml
/pet:
  post:
    tags:
    - "pet"
    summary: "Add a new pet to the store"
```

#### RAML (`v1.0`)

Add method level tags using the `tags` annotation as shown below:

```yaml
/pet/{id}:
  get:
    (tags):
      - pets
    displayName: Get pet by id
```

More details about this annotation and its usage can be found [here](https://docs.apimatic.io/advanced/raml-apimatic-annotations/#annotation-for-method-level-tags).