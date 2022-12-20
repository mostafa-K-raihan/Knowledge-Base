<span style="color:red">BUG</span>
- ~~What if user deletes Webpage Analytics component from the settings page? then the guid that resides in org model will not find the content type, will get a 404
- ~~If you uncheck module access, webpage analytics property from org model (scAnalyticsContentTypeGuid) should reset, it isnt right now.~~ talked with F bhai, seems like we want to have whatever it is now.
	- there is a faulty test for it, thats why the bug is not caught yet, the test assumes, before anything the guid is not available, the test should assume instead, previously the guid is available, and when moduleAccess turned off, the guid should reset.
- ~~When querying the content types, we also fetch content types from external channels like AEM, cms12, if any of this two fails, whole promise is rejected. because these are wrapped in Promise.all, also, it make no sense to query for cms content types, if the instance does not have cms integration. https://github.com/newscred/cmp-server/pull/23363
- There is a big space in the inline editor, at the very bottom of the content
- When we hit preview, there is a brief period of time, we show EmptyState, then we load spinner, and in the background some of the message is also shown which is not ideal. (try zooming out in non previewable content)
- 
<span style="color: yellow">TECH DEBT</span>
- Fix file structure of structured-content
- (Future) - Allow CMS Content Type (Wrapper) to be not deleted.
<span style="color:green">New Features</span>
- Paginate `getContentTypes`
	- restrict number of content types returned by the API
	- so that Add Content Dropdown is not overloaded
	- settings page can have reasonable heights etc.
- Support pressing enter in our forms

<span style='color:orange'>SPIKES</span>
- Investigate if we can use `expanded: false` when we fetch contents.
- Can we reduce our payload? Do we need all the fields that are sent by content-repo, can graphql help?
- Will it help performance if we could use react query instead of regular one when we fetch content types/type/content?
- Reduce logic written in front end?
- Centralize types in one place
- 