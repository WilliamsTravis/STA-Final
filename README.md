# STA-Final
   
   For the final project in Space-Time Analytics I will be adding to an Agent Based Model of rancher stocking response to drought. The program Agent Analyst is used as an interface between the ABM program Repast and ArcGIS. It allows Repast to access data in the GIS, create
agents and environments out of that, apply action and scheduling code that alters the data, then overwrite the data in the GIS and update the display for a continuous presentation of the phenomena that the ABM is simulating. 
   The phenomenon we are looking at is the adoption of drought insurance by ranchers in response to drought. This is a precipitation-index based insurance product made available through the USDA's Risk Management Agency last year as means of supporting feed costs and mitigating against financial loss from lost grassland forage potential and reductions in cattle stock. The decision to purchase insurance will be based partly on simulated behaviors (that can verified or updated with actual data in the future) and actual data derived from historical precipitation records and cattle sale data from the last 10 years. The decision to purchase the insurance is considered to be based on need, with the largest indicator of such, in the data available to us, at least, being cattle sales. 
   It is expected that there is a relationship between rainfall and cattle sales, with rising sales in response to dry conditions. This relationship will be the foundation of a decision function that each simulated rancher will use to decide to adopt insurance, which, once purchased, will provide a dampening effect on the amount of cattle that must be sold. Other factors and the conceptual code are described below:
   
Each time-step (1 months), each polygon agent will: 

-assess their average precipitation levels and compare it to the average baseline precipitation (the “strike-level” that triggers   payout) to see if they would have been eligible for a payout; 

If precip < strike:
coulda_count = 1
coulda_count = coulda_count + 1
		
-look back and see how many times they could have had a payout and if it reaches a certain ratio we could add that as a decision weight for whether or not to buy insurance. 
			
		If coulda_count / timestep_count > .5 (any threshold ratio):
			woulda_count = woulda_count + 1 ( adding to our weight counter)
			
-Assess what ratio of neighbors have taken out insurance this year, or still has insurance this year, to the overall number of neighbors. Then, if the ratio reaches a certain threshold we will add weight of neighborhood to the decision equation. 

		If neighbors_did > .5 (or any threshold ratio):
			shoulda_count = shoulda_count.timestep[:-2] + 1
(We could also base this on a ratio, as in the more neighbors who have it the more likely the rancher is to buy it)

-Assess the amount of precipitation, and adjust their stocking rate accordingly by selling cows, buying cows or doing nothing. If he has to sell off a certain ratio of his stock we will add a weight of lost revenue to the decision equation. We will have to find an equation that tells us how much cattle they would sell at each at each precipitation level (drought_effect). This may not be  field, we will probably need to include the equation 

		stock = stock + (roundup(stock * (drought_effect)/stock)
			If roundup(stock * (drought_effect)/stock  >  5 (a loss_tolerance):
				Stockweight_count = stockweight_count + 1
			
-If the agent has insurance, it will provide a dampening effect on the amount of stock he must sell, or might also add to how much he buys . If we have time we could use this to decide whether or not he would buy better insurance (higher strike-level and liability)

-Finally, take into account his situation, weigh his options (buy or not buy) and decide whether or not to do it. Also, maybe how much insurance to take out and at what strike level.
	
	Maybe_should = some function of (woulda_count,shoulda_count and
 Stockweight_count)


If maybe_should > thats_it:
	Self.insured = True

    

