# Task assigned by recruiter.

## Task ID: 1Hg5IooAWCmCGQu04cXAuevjFTeV6tdikA9oQmbwfD5k
### Fetched from below source
https://docs.google.com/document/d/1Hg5IooAWCmCGQu04cXAuevjFTeV6tdikA9oQmbwfD5k/edit#

## PREAMBULA

InComm (client) helps partners (like Apple, Starbucks, etc) sell their gift cards (https://en.wikipedia.org/wiki/Gift_card) in retail stores (like Walmart, Target, etc - they are called merchants). Gift card stands in a retail store.

InComm has 200 merchants and 700 partners. Each merchant and partner can have from 1 to 25,000 locations. An average number of locations is 1,000 per partner or merchant. So InComm covers 200,000 merchant locations and 700,000 partner locations across the USA.

InComm sales team (users) runs presentations to new and existing partners where they show a report how many merchant locations are located within a certain radius from the partner locations. This number is important as it impacts business decision for a partner whether it makes sense for them to sell their gift cards through specific merchants.

For example, 99% of Highs of Baltimore (merchant) locations are within 25 miles from Bruster's Ice Cream (partner) locations. But only 47% of Thornton's (merchant) locations are within the same radius from Bruster's Ice Cream (partner) locations.
 
Based on this mapping (this is how they call the calculation above) the partner can make a decision to sell through Highs of Baltimore and skip Thornton's.

In order to generate a report like that InComm sales team picks a partner and chooses a radius for the mapping:
<pre>
    Start new mapping
    Partner [ Bruster's Ice Cream     ▼ ]
    Radius  [ 25 miles                ▼ ]
    ...     ...
                        [ Start mapping ]
</pre>
As a result, they get a list of merchants with their coverages (coverage is percentage of merchant locations within a certain radius from partner locations) for the radius they set:

InComm sales team uploads locations for a specific merchant or partner into the system once or twice and then generate mappings many times. So location upload is a rare operation and mapping is a frequent operation.

### Map

Along with coverages InComm sales team needs to see locations on the map. The map shows all partner locations (red) and all merchant locations (various colors) within the radius:

We should use Google Maps with our own tile server. Tile Server is a module that generates tiles for Google Maps. Tiles are images with markers on them like this:

Google Maps script that we embed into our page has an interface to point it to our tile server and once we provide URL to the tile server Google Maps starts requesting tiles from it and apply them on top of the map. This is how all the locations are shown on the map.

There are around 10 tiles covering USA:
 
InComm sales team can select and unselect any subset of merchants from the mapping and show only selected merchants of the map:

## REQUIREMENTS

InComm sales team should be able to generate coverages within 1 second and draw the map within 10 seconds.

## OUTCOME

You need to design this system from software architecture standpoint:

1.	Break down on applications, APIs, services, servers, databases, etc. 
2.	Explain purpose of each architecture element
