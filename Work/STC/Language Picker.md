- Create different content / locale
	- Parameterize create / version endpoint to support language (cmp-server)
	- when creating a new content with a new language, we need to create a relation between these two asset, to show them in library related asset
	- add an option `Create Translation` in the context menu (under FF of course), clicking that will open a new modal containing language picker, (dropdown with language endpoint result)
	- selecting a language will create a new content with that language and create a relation?


**Design Q**
- when we create a translation from an existing asset, translated asset is related to that existing asset. is it vice versa? like if we create a french translation from an english one, what will the user see in the library > related asset section when the user clicks on the french asset
- we can remove relationship from related asset section, will that be the case for related assets based on language, if so, those assets are no longer related in library, what about in task level
- can we change the title of the translated assets
- if so, what will the dropdown show as a label for grouped asset
- is creating fr from eng and then swed from eng the same as creating fr from eng, and swed from fr?
- if we undo a task, will the relationship in library be discarded and not in task level?

- wrapper sc (translated from (english))
	- related asset:
		- created asset guid (swedish)
- wrapper sc (translated to (swedish))
	- related asset
		- existing asset guid(english)
	-

how do we group related assets in task level
- loop thru all the assets (based on whatever sorted way we receive from the server)
- if it has related asset section, discard those guids from the assets iterable
- and group the asset
ex
	we have
	- eng
	- then from eng we created fr
	- then from eng we created bd
	- regular imag
- eng => related asset fr, bd
- fr => related asset eng, bd
- bd => related asset eng, fr
- loop thru 4 asset
- first found eng, saw two guids that are in related
	- create a dummy object/asset (which leaves in memory/client state) basically holding the group info and discard those two guids from the iterable
- second found a regular image, since the two other scs are discarded already, when we are processing the first item
- so we have two assets to show, one is group and the other is regular
- group asset is shown according to the design with a stacked image
- when clicked the view gets changed, we introduce a dropdown with the title of base asset, with all the asset guids that are related.