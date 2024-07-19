# Targeted Updated to Active Travel network

### Motivation:
Transport agencies typically have limited resources to encourage active travel I their cities and to make change to the built environment to encourage and improve cycling potential. Using properties of the network can we identify which are the optimal changes to make to a network in order to improve cycling potential?

### What we know:
- People make decisions around cycling and route choices which trade off a variety of factors such as directness, comfort, safety etc
- For a single OD pair these can be represented on a pareto curve of all possible routes – which is called the bikeability curve in Regianni et al (2021)
- The shape of the curve determines to what extent the network supports cycling for a range of different users between the OD pair
- We can measure the “trade off rate” which tells us how much discomfort is traded off for each new route discovered on the pareto curve – a high trade off rate is likely to indicate a network that doesn’t accommodate a wide range of different users
- Features of the built environment can be perceived, to some extent, from OSM data and related to the cyclability of an edge e.g., through measuring LTS
- The betweenness centrality of an edge indicates it's importance in routing operations in the network. Networks which have cycle friendly edges with high centrality also attract higher number of cyclists (results still TBC)

### Possible approaches

This can be considered from the demand perspective or the supply perspective.

From the supply perspective we may think of it as "inefficient supply". An example of this is a network with a high average trade off rates between crticial OD pairs.
From the demand perspective we may think of it as "unmet demand". If we know behavioural patterns and underlying demographic distributions, we can identify areas where we may expect there to be higher rates of cyclability.

Both cases can suggest to us area, in a broad sense, of the network where interventions can be impactful.

However, the crux of the problem is identifying the specific edges which are restricting the most amount of active travel potential. These ideas can help guide us to a technical approach.

### Data

- OSM data with well performing models such as Ottawa LTS
- Census data which provides
  - Demographic distributions e.g. at LSOA level
  - OD flows (MSOA level)
- CSNA audit rating in certain areas
- Strava data indicating commuter flows
- Granular flow data in London*
- Satellite imagery*

* Data exploration and understanding of usefulness tbc

### Possible approaches

Optimisation problem - minimise "trade off rate" (ToR) at the network level within a given budget constraint
(Alternative suggestion : maximise weigted LTS centrality)

Can we develop a simple heuristic to solve this problem? As a quick suggestion:
- Make update to network
- Measure impact on objective function (mean ToR)
- Use objective function change and some mapping to the network / OD pairs to suggest where a better intevetion may exist
- Update network according to this measure and repeat until convergence

### Proposed Technical Approaches

#### Genetric algorithm:
- Generate sparsified representation of network
  - E.g. OA to OA with links representing shortest path on LTS 1,2,3,4 edges
- Genes link-level LTS penalty e.g., penalty for moving from LTS4 -> 3, 3 -> 2 etc
- System wide objective function to test/minise mean ToR

Still to solve: the algorithm will probably eventually converge on all links having 0 penalties as this is the optimal configuration. We want the algorithm to be constrained e.g., by budget or some realistic measure of what can actually be changed by a transport agency.

Such an algorithm will suggest which connections in the network to modidy to improve network-wide bikeability, in other words ensuring that the network can provide safe routes for a diverse set of potential users.

Limitations: the method doesn't account for useful trip e.g., do the changes actually enable connectivity to useful places? The method doesn't account for demand.

#### Reinforcement Learning:
- Define environment as road network with safety features
- State: cycle flow through network, exposure to risk, circuity
- Agent adjusts LTS levels on each road
- Reward function: average exposure to risk, average circuity, mean ToR, LTS centrality etc


### Resources

* Reggiani, Giulia, et al. "Understanding bikeability: a methodology to assess urban networks." Transportation 49.3 (2022): 897-925.
* Mahfouz, Hussein, Robin Lovelace, and Elsa Arcaute. "A road segment prioritization approach for cycling infrastructure." Journal of transport geography 113 (2023): 103715.
* Paulsen, Mads, and Jeppe Rich. "Societally optimal expansion of bicycle networks." Transportation research part B: methodological 174 (2023): 102778.
* Vybornova, Anastassia, et al. "Automated detection of missing links in bicycle networks." Geographical Analysis 55.2 (2023): 239-267.

Relevant Github Repos:
* https://github.com/madspDTU/OptimalBicycleExpansion (related to Paulsen et al paper)

