## To Do.todo

* Get Create working again. @done(2016-01-25)
* Add structs.API to Services request.
* Change request/common.go: error check validate()... do not error check body or query parm conversion.
* Implement report searches:
	* Single ID
	* DeviceID
	* LatLng
	* Address
* Update RAML file with JSON specs for input and output payloads.
* Update Services cache on Engine. @done(2016-01-22)
	* Be able to safely and easily refresh the services list. @done(2016-01-22)
	* Quickly lookup location -> AreaID -> Service list. @done(2016-01-22)
	* Quickly lookup location -> AreaID -> Adapter list. @done(2016-01-22)
* Implement RPC Dispatch System @done(2016-01-19)
* Custom Type for ServiceID. @done(2016-01-11)
* CitySourced Adapter - load config.json data file. @done(2016-01-05)
* Separate back-end interfaces as separate apps, using go-rpc calls. @done(2016-01-05)
* Implement Gingko BDD tests. @done(2015-12-18)
* Modify report.Create() to use ServiceID and JID. @done(2015-12-28)
* Consolidate all Create functionality within the request.CreateReq type. @done(2015-12-28)
* Implement Services API endpoint. @done(2015-12-18)
	* Bring Google Maps API over. @done(2015-12-18)
* Modify CitySourced simulator to return the Request ID and Document ID. @done(2015-12-12)
* Outline the Displatch system. @done(2015-12-14)

___ 

## Log

### 2016.01.27 - Wed

* Revised RPC management in engine/router.  The router is much more self contained, and will properly route requests to the proper Adapter based solely on information within the RPCCall struct.  
* Test OK.
* Saved to GIT.
* Changed IFID to AdpID everywhere for clarity.
* Deleted unused, commented code.
* Removed some Debug logs.
* Test OK.
* Saved to GIT.
* More debug print cleanup.
* Saved to GIT.
* More cleanup:
	* Removed "arith" sample code from Adapter code.
	* Cleaned up all import statements.
	* In engine/router/rpc.go, renamed RPCCall.listIF to RPCCall.adpList.  This is in line with the terminology and naming elsewhere in the project.
* Added color coding capability to common.LogString and log.LogString.
* Test OK
* Saved to GIT.


### 2016.01.25 - Mon

* Create is working.
* Saved to GIT.

### 2016.01.22 - Fri

* Modified services/services.go to create a map of AdapterIDs by AreaID, and send it back to router/adapters.go for updating.
* See new doc: "channels.md" for details.
* The adapters are frequently referenced by AdapterID, so I changed engine/data/config.json to use objects for each Area, rather than a list.
* Changed Adapters.Name to Adapters.ID in router/adapters.go, to match the current usage and docs.
* Test OK.
* Saved to GIT.


### 2016.01.21 - Thu

* Modified ServicesList refresh to go through a channel, so that only one update can possibly be running simultaneously.  To start a refresh, call router.RefreshServicesList().
* Saved to GIT.
* Moved services.go from the engine/router package into it's own, new package "services".  All code related to the ServiceList cache system will be in this package.
* The services package needs access to the RPC call system, so revised engine/router/rpc.go - made the following exported:
	* newRPCCall -> NewRPCCall
	* rpcCalls -> RPCCall
	* rpcCalls.run() -> RPCCall.Run()
* Restored engine/request/services.go, and updated to match all the changes in the Engine.
* Test OK.
* Saved to GIT.
* Renamed engine/router/adapters.go -> data.go.  There is more in the config.json file than just adapter data.
* Fixed Services response.
* Test OK from Paw client, with location in San Jose.
* Saved to GIT.
* 


### 2016.01.20 - Wed

* Service List update working better.  Needs better locking... multiple accesses are possible now.

### 2016.01.19 - Tue

* Changed all variables and types name "city" to "area".  For now, "areas" correspond to cities, but there is a good chance that will change in the future.  
* Got Humpty Dumpty put back together.  RPC dispatch system is now working.
* RPC Dispatch System is in new file engine/router/rpc.go.
* Saved to GIT.

### 2016.01.15 - Fri

* Services List working from Engine and Adapter (Citysourced)!.
* Set up 2 Adapters (CS1 and CS2) for testing.
	* CS1 = San Jose
	* CS2 = Santa Clara
* Removed the LogString (for boxed struct printing) from logs.  It was already in common.  Updated the code in common with comments, and removed the LogPrinter stuff (not used).
* Added command line args (flags) to the Engine, as well as SignalHandlers.
* Saved to GIT.

### 2016.01.14 - Thu

* Implemented go-logging in adapters/citysourced.
	* Converted all fmt.Print statements to log statements.
* Command line options working now.
* Signal handler working.
* Saved to GIT.
* Renamed functions in adapters/citysourced/data.go to show they are about Services:
	* City() -> ServicesCity()
	* All() -> ServicesAll()
* Removed debug from config file.  It's redundant and unnecessary.
* Saved to GIT.


### 2016.01.13 - Wed

* Renamed directories:
	* gateway -> engine
	* integration -> adapters
* Started rebuilding the Engine.
* Saved to GIT.
* 


### 2016.01.12 - Tue

* In integrations/citysourced/request/services.go:
	* Change Service.ServicesForCity() to City().
	* Added Service.All() - returns a list of ALL services.

### 2016.01.11 - Mon

* In integration/citysourced/data/data.go;
	* All indexes working now.
	* Deleted the "serviceProvider" index... it is no longer needed with the new Master Service IDs.
	* Added a Santa Clara section of test data in config.json.
* Moved the structs.go file into it's own package: integration/citysourced/structs/structs.go.  Had a circular import issue.
* Saved to GIT.
* RPC test working OK with Services API.
* Saved to GIT.
* Added second CitySourced integration for testing purpose.
* Saved to GIT.
* Changed gateway/request/*.go:
	* All fields need to be exported in the Create structs, so change "longitude", "latitude", etc. to "LongitudeV", "LatitudeV", etc.
* Moved gateway/request/structs.go to gateway/structs/structs.go
	* Added gateway/structs/methods.go.  
* 
	
### 2016.01.07 - Thu

* Tried an alternative layout using the ID's as keys in the Adapter/config.json file.  This (theoretically) would have been convenient, but the JSON decoder doesn't support anything but strings for the "keys".  Not working well.   
* Revised config.json again... switched back to mostly lists.
* integration/citysourced/data/data.go working.  Data being loaded successfully.  Service list is of the NService type, so it will be very efficient to look up and return to the Engine.
* Test OK, for what it is.
* Saved to GIT.

### 2016.01.06 - Wed

* Worked out JSON marshalling for the Service ID custom type.
* Need to rearrange the citysourced/data/config.json file again... need to expand the Area (City) section.
* Saved to GIT.
* Rearranged citysourced/data/config.json.   

### 2016.01.05 - Tue

* Wrote up "EngineAndAdapters.md" file outlining design of the Engine / Adapter system.
* Saved to GIT.
* Changed _Test/CSclient.go to use asynchronous API calls.  Working OK.
* Saved to GIT.

### 2016.01.04 - Mon

* Created separate directories for the Engine and Adapters, and separated the existing source code.
* Saved to GIT.
* Worked on design of Engine/Adapter RPC system - responsibilities, design.
* Got the CitySourced client rudimentarily working.  
* Put all "Native" structs into a the request/structs.go file.
* 

### 2015.12.30 - Wed

* Renamed geo functions:
	* LatLngForAddr() -> LatLngForAddr()
	* AddrForLatLng() -> AddrForLatLng()
	* CityForLatLng() -> cityForLatLng()
	

### 2015.12.29 - Tue

* In integration/citysourced.go, created Search structs:
	* CSSearchLLReq - search by Lat/Lng
	* CSSearchDIDReq - search by Device ID
	* CSSearchZipReq - search by a Zip Code
* Created request/search.go:


---
#### How to Handle Dispatching Searches

**Search by LatLng**

* For now, find the city for the Lat/Lng coordinates, and send a search to all service providers for that city, with the specified coordinates and radius.
* *Limit the radius - 100m?*
* __Recipe__
	* Get City

**Search by Device ID**

* If the request includes a list of previous Service Provider ID's, then use it.
* Use the current location (or specified address), and query all Service Providers for that City for the Device ID.

**Search by Zip**

* Easy with CitySourced...

---

### 2015.12.28 - Mon

* Greatly reorganized request processing to Create a report (in the "request" package):
	* Renamed:
		* CreateReport type -> CreateReq
		* CreateReportResp type -> CreateResp
	* Moved all Create functionality under the CreateReq type - current methods:
		1. validate
		2. init
		3. parseQP
		4. run
		5. ProcessCS (processes against CitySourced backend)
		6. String
		7. toCS (converts CreateReq struct to citysourced.CSReport type)
	* Moved all of the above Create types into the new file: "request/create.go".
	* Added "apiVersion" to config.json in the Provider section, and also added to the Provider type in "router/data.go".
	* Saved to GIT.
	* Test OK
	* Saved to GIT.
	* Removed JID from Create URL.  Also removed from use in Create struct, etc.
	* Moved the common code (cType and cIFace) from request/report.go to new file request/common.go.
	* In request/common.go, discontinued use of the "inputBody" and "inputQP" fields.  In cType.init(), will always attempt to decode payload, and parse query parms.  Simplifies the code.
	* Test OK.
	* Saved to GIT.

---
Thoughts on using the JID for most/all requests:

* The JID identifies the City ("Jurisdiction").  _This may become more finely grained using some type of non-overlapping jurisdictional map areas that do not necessarily match city boundaries._
* Requsts that return the Jurisdiction ID:
	* Service List
* Requests that would use the current Jurisdiction ID:
	* Create _(this can use Service ID to get the Jurisdiction ID)_
	* Search by DeviceID _(if the App cannot provide a list of previous Jurisdiction ID's)_
* Requests that would not necessarily use the current Jurisdiction:
	* Search by Current Location _(we need the exact lat/lng)_
	* Search for an Address
---

* Reorganized the Services request (get a list of available services for the specified location):
	* Created request/services.go.
	* Used Create as a template.
	* Created ServicesReq and ServicesResp types.
	* Encapsulated all services retrieval functionality inside the ServicesReq type.
* Modified request/common.go:
	* Changed init() to load().  This will remove the conflict with the parent init().
	* Test for error: "JSON payload is empty".  TODO: This needs to be cleaned up... all checking should fall on the validate() function, and the body and query parm parsing errors should be ignored.
* Renamed request/report.go to request.go, as this is the primary file in the request package.
* Test OK.
* Saved to GIT.
		

### 2015.12.23 - Wed

* Cleaned up router.ServiceProviderInterface().  Added test cases in data_test.go.
* Test OK.
* 

### 2015.12.19 - Sat

* In router/data.go:
	* Dropped "Getxxx" from the ServiceXXX() methods.
	* Added ServiceProviderInterface() go get the Service Provider interface type (currently on CitySourced).  
* in request/report.go, added a map lookup (beCreate) on the Create functions.  We will have a set of these maps to quickly route an incoming request (create, lookup, etc), to the correct backend interface.
* Test OK.
* Saved to GIT.

### 2015.12.18 - Fri

* Brought "geo" package over from CitySourced.
* Added "getCity()" function to mygeocode.go.  This scans through the Google response and retrieves the city.  We will need this for quickly mapping the Mobile Apps geoloc -> city -> Service Providers -> list of Services.
* Test OK.
* Saved to GIT.
* Added a CityForLatLng() func in the "geo" package.  This takes a latitude and longitude, and returns the City.
* Test OK.
* Saved to GIT.
* "/services" endpoint working - returning the list of services for San Jose.  Returns 500 for city: "Morgan Hill", with error: The city "Morgan Hill" is not serviced by this Gateway.
* Test OK.
* Saved to GIT.
* Added Gingko test suite.
* Revised router/data.go:
	* Moved Services() and ServiceProviders() to be methods of RouteData.
	* ServiceProviders() now returns the Provider list, and an error.
* Test OK.
* Saved to GIT.


### 2015.12.17 - Thu

* Router data and indices are working.
* Test OK.
* Saved to GIT.
* Split the test data into two Service Providers for San Jose to test multiple providers.  
* Added providerService to the RouteData struct.  This implements a map of the ServiceID back to the Service Provider.  This provides a fast lookup of the appropriate provider for a New Report, based on the ServiceID.
* Test OK.
* Saved to GIT.

### 2015.12.16 - Wed

* Completely reformatted JSON file... made Services a direct child of Service Provider, and Service Provider is a child element of Service Areas.  Go will load this as a series of maps, which will automatically give us some fast indexing into the data.

### 2015.12.15 - Tue

* Updated ServiceRouter.md documentation.
* Created 

### 2015.12.14 - Mon

* Added design documentation for the Service Router capabilities.  
* Saved to GIT.

### 2015.12.10 - Thu

* Doing other things for a few days.  Coursework, etc.  Back on the case now!
* Current status: "Create" is working for CitySourced API. 
	* Current libraries
		* github.com/ant0ine/go-json-rest/rest - this is working well for the front-end, but does not support XML, and so will not work for the backend.
	* No routing/dispatching is in place yet...  This is probably the next task.

### 2015.12.04 - Fri

* Implemented "napping" REST client.  Not working due to XML.  Starting over using just the HTTP lib.
* Saved to GIT.
* 

### 2015.12.01 - Tue

* 

### 2015.11.30 - Mon

* Wrote up 311 Gateway Proposal.

### 2015.10.27 - Fri

* Reviewed RAML spec with Hassan.  Discussed the Gateway idea.

### 2015.10.24 - Tue

* Revised RAML spec.

### 2015.10.23 - Mon

* First draft of RAML spec.
* Posted to Slack.
