### Create Content
- POST`/v2/organizations/{organizationId}/integrations/{integrationId}/cms/tasks/{taskId}`
	- query params are
		- languageId
		- parentId - what is it??
		- contentTypeId
	- returns `ContentCreated`
		- `id` - assuming content id?
		- `languageId` - why do we need this? its not that we request to create french content and receives a spanish one, right?
		- `editUrl` - where should we store this? add a new field definition in wrapper?
		- why no version id?

### Edit Content
- POST`/v2/organizations/{organizationId}/integrations/{integrationId}/cms/tasks/{taskId}/{contentId}/{languageId}`
	- we are really gonna put 3 guids at the end?? without specifying resource type?
	- returns `ContentCreated`
		- same things as Create Content, no version id
- DELETE`/v2/organizations/{organizationId}/integrations/{integrationId}/cms/tasks/{taskId}/{contentId}/{languageId}`
	- same thing as above?

### Get ContentTypes
- GET`/v2/organizations/{organizationId}/integrations/{integrationId}/cms/contenttypes`
	- query
		- `parentId`
	- returns `ContentType`
		- id
		- name

### Get Languages
- GET`/v2/organizations/{organizationId}/integrations/{integrationId}/cms/languages`
	- query
		- parentId
	- returns `Language`
		- id
		- name

### Get Structure - what is it? for destination picker?
- GET`/v2/organizations/{organizationId}/integrations/{integrationId}/cms/structure`
	- query
		- `end` - what is it?
	- returns `ContentNode`
		- `id`: `string`
		- `name`: `string`
		- `hasChildren`: `boolean`
		- `children`: `ContentNode[]`

### Get Structure By Node - what is it? under a specific destination/folder?
- GET`/v2/organizations/{organizationId}/integrations/{integrationId}/cms/structure/{rootId}`
	- query
		- `end`
	- returns `ContentNode`
		- `id`: `string`
		- `name`: `string`
		- `hasChildren`: `boolean`
		- `children`: `ContentNode[]`

In all cases the response code is either
- 200 or 400
- if 400 we get `problemDetails`
	- `type`: `string`
	- `title`: `string`
	- `status`: `integer`
	- `detail`: `string`
	- `instance`: `string`