POST - create structured content
- ~~do not create nested content~~ (Done)
- refactor formatting code in asset

### Recommendation
- Can we make creation of assets in the background, it does not affect the response of original POST call (reduction possibility ~ 5s)
	- what if the back ground call fails?
- Merge Fetch Text Content and Fetch Structured Content (reduction possibility ~1.5s)
	- Inside Fetch Text Content we are fetching SC again
	- 3.6s => 2s?
- Can we speed up setting webpage analytics somehow? (reduction possibility ~ 1.3s)
	- it fetches structured content with webpage analytics guid
	- maybe pass a global dictionary to store the created sc, so that we no longer have to fetch again, 