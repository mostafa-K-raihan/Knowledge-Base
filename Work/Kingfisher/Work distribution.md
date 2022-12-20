- Create a CMS12 content type for each organization
	- When a checkbox is toggled to ON, run some code to create a CMS12 content type
		- if it is already created just bump the version
	- save the content type guid to org model, same as webpage analytics
- Create an empty content whenever the requested content type guid == CMS12 content type guid

# Clarification needed
- what is integration id: `channel._id`
> We store ContentID+Language in SC placeholder (ProjectID and VersionID will be infered by task/context)

where should the language param reside
	- root level/field level
	
> Note that there will one SC placeholder per language (in practice a language version in CMS)
		
>  If multiple content exists within a task they can be previewed together

How?

> To support personalization preview the user needs to select which visitor groups to preview

What is personalization?

>  The project within CMS that is connected to the task will be published, i.e. all included versions in the project will be published (pages, blocks etc)

We have a concept of selective publishing, meaning we can control which assets to publish, how will that work? for ex. lets say we add a french translation of an existing CMS content. according to above ^^^^ we will have 2 different SC for this(one SC placeholder per language), but in CMS end it will have different version, now the author wants to publish english not french, how will this work based on the current statement(all included versions in the project will be published)?

> Required input from the UI:
> 	The task that should be published

As above, we need to pass the content guids that needs to be published as well.