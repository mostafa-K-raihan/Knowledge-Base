- Figure out why sqlalchemy takes about 1.3 seconds to fetch content 
- Explore progressive loading possiblities
	- client should send hydration depth = n (default = 1), based on that server will respond accordingly
	- when client requests for full content, server should send hydration depth = 1, and a parital content 206 response, then client will send subsequent requests untill we get 200 
	- when client requests for full content, server should send hydration depth = 1, and 200 response. nested contents will fetched when the user is actually having an interaction like expanding the accordion. Q - When will we load the webpage analytics, it is a nested content, and not visible for the client side
- Do similar things in the regular SC endpoint as the CMS endpoint, like not trigger a fetch call after we receive a 200 from the POST call
	- need a refactor in the server side

# TODO
- define flows
- ~~bring back fetch Content calls~~ done
- ~~stop calling = true from asset~~ done
- parallelize webpage analytics and get content call from asset
- parallelize preview and get call somehow?
- ~~creation of assets in the background? UI does not care about it, right?~~ can't because of permission
- ~~we can however pass additional meta info like title, version guid, so that we no longer have to call~~ done
- and we can deprioritize the webpage analytics version bump call later
- Make a POC under a FF
- Make a dashboard to track our perf work
- add a profiler script to run in the prod
- use false for fetching content from server, and make sure it works in the client, we no longer creating nested stuff, so it does not make sense to fetch everything, they will be empty anyway
- but interesting stuff will happen if we edit a nested content, that will create a nested content, we should add a progressive loader or something
- 

Dashboard
- content repo 
	- "POST create content" OR "Successfully created content" - creation latency ~120ms
	- "Fetching content" OR "Creating content" - db get content latency ~1.6s => 350ms
	- Creation Latency 3.6 total 3.2 for getting content, 400ms for rest