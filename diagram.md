# Development of mapping and coverages evaluation system

## PROBLEM

Estimated rows processed by earth_distance (psql 9.4+) 2.2 million records processed per 22ms
Which means 100 million per second = 1e8 s^-1

<pre>
⸝-----------------------------------------⸜
| ‾‾‾⸌ᐧ---⸜___ |   partners  |  merchants  |
|-------------|---------------------------|
|   number    |     700     |    200      |
|-------------|---------------------------|
|  locations  |             |             |
|   average:  |    1000     |    1000     |
|   maximum:  |    25000    |    25000    |
|-------------|---------------------------|
|  estimated  |             |             |
|    total    |   700 000   |   200 000   |
|  locations  |             |             |
|-------------|---------------------------|
</pre>

Estimated average query for coverage of each partner:
1. with average merchant: 1000 * 1000 / 1e8 = 0.01s
2. with major merchant: 1000 * 25 000 / 1e8 = 0.25s
3. with all merchants: 1000 * 200 000 / 1e8 = 2.00s
* Apply safety margin for every value = 4.0
* Quote: "sales team should be able to generate coverages within 1 second" 
* Assume that there will be only one concurrent request from sales team at a time

Estimation #3 is critical, what can take 2.00s must securely take 0.25s. This leads to a next:
Conclusion: We have to split computation of radiuses (earth_distance) between at least 8 concurrent database replication services

### Diagram of updates to merchant locations 

![Frontend for Backend location updating diagram](http://www.plantuml.com/plantuml/proxy?cache=no&idx=0&src=https://raw.githubusercontent.com/PapevisO/1Hg5IooAWCmCGQu04cXAuevjFTeV6tdikA9oQmbwfD5k/master/diagram.puml)

### Diagram of daemon processing merchants locations records

![Backend for Backend location updating diagram](http://www.plantuml.com/plantuml/proxy?cache=no&idx=1&src=https://raw.githubusercontent.com/PapevisO/1Hg5IooAWCmCGQu04cXAuevjFTeV6tdikA9oQmbwfD5k/master/diagram.puml)

### Diagram of mapping service application

![Backend for Backend location updating diagram](http://www.plantuml.com/plantuml/proxy?cache=no&idx=2&src=https://raw.githubusercontent.com/PapevisO/1Hg5IooAWCmCGQu04cXAuevjFTeV6tdikA9oQmbwfD5k/master/diagram.puml)
