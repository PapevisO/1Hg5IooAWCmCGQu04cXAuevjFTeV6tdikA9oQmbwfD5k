# Development of mapping and coverages evaluation system

## PROBLEM

estimated rows processed by earth_distance (psql 9.4+) per 22ms : 2.2e6 ~ 100M/s

<pre>
|-----------------------------------------|
|      \_     |   partners  |  merchants  |
|-------------|---------------------------|
|   number    |     700     |    200      |
|-------------|---------------------------|
|   average   |             |             |
|  locations  |     1000    |    1000     |
|-------------|---------------------------|
|  estimated  |             |             |
|    total    |   700 000   |   200 000   |
|  locations  |             |             |
|-------------|---------------------------|
</pre>


![Frontend for Backend location updating diagram](http://www.plantuml.com/plantuml/proxy?cache=no&idx=0&src=https://raw.githubusercontent.com/PapevisO/1Hg5IooAWCmCGQu04cXAuevjFTeV6tdikA9oQmbwfD5k/master/diagram.puml)

![Backend for Backend location updating diagram](http://www.plantuml.com/plantuml/proxy?cache=no&idx=1&src=https://raw.githubusercontent.com/PapevisO/1Hg5IooAWCmCGQu04cXAuevjFTeV6tdikA9oQmbwfD5k/master/diagram.puml)
