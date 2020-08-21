# Facebook-social-network-analysis

Data source: https://snap.stanford.edu/data/ego-Facebook.html
"This dataset consists of 'circles' (or 'friends lists') from Facebook. Facebook data was collected from survey participants using this Facebook app. The dataset includes node features (profiles), circles, and ego networks."

## Project Description:
My project is to explore the structures of a typical Facebook friends circle. The dataset that I will be using is one of the ten sub-circle (the first friend circle) from the combined version of the large circle. Each sub circle was settled by the data contributors, that I'm not responsible for spliting the data from the combined version. So, analyzing the social network structure of the typical friend circle will also helps me understand why and what criteria the data contributor follows to split the data. My interests include how many clusters appears in the first friend circle and what are the major (statistically significant) attributes (demographic information, etc) that are associated with the social network. 


## Methods:

### 1. Data transformation:
The first challenge is that the features are labeled anonymized for facebook users, since the names of the features would reveal private data. Each of the sub-circle includes three datasets - "featnames", "feat", and "edge". I will use the "edge"(edgelist) to construct the adjacency matrix "A", and use the combination of "featnames" and "feature" to construct an attribute table "feature". 

The dataset featnames refers to the names of each of the feature dimensions, which explains each feature the column from the "feat" table represents for. Note that you can see many "duplicated" feature names shown on the featnames. My assumption is that since the data intro that each feature column are binary (features are '1' if the user has this property in their profile, and '0' otherwise), one column won't include enough info if the question is not merely asking "did you go to college or not", but is asking "which college did you go to".

In other words, columns that have the same name such as "birthday;anonymized" can be like: column 1 - January; column 2 - Feburary, etc. Columns that are all named "education;school;id;anonymized" can be like: column 10 - UCLA, column 11, USC, etc. 

The second problem is that each column only has a few 1's. So, if we just use the "feat" table, it is a dataset with 225 columns, and each column has only a few 1's - we won't be able to explore many useful info based on the original feature data.

My method is to rescale the data sequence from the columns that have the same name and sum them up. For example, if column 10 to 15 represent "education;school;id;anonymized": I will keep the binary values in column 10 as 0 and 1, but rescale column 11 to have 0 and 2, and rescale column 12 to have 0 and 3, etc. Then, I will add them up and have this one final column that has values (0,1,2,3,4,5). 0 indicates the user didin't go to any universities listed from the survey, 1 to 5 indicates the user goes to different universities accordingly. One potential risk would occur if column 10 - 15 include schools from different level - column 10 refers to "high school A", column 11 refers to "college A", and column 12 refers to "college B". In this case, if I just sum them up, and people who went to high school A also went to college A, then 1+2 =3 would be the same with the value 3 in column 12, which indicates people who went to "college B". Then this method could not distinguis the user ideally. Without enough data introduction, this is the best I could do at this moment.
 
### 2. Social network analysis

1. Convert the data from edgelist to adjacency matrix using igraph package.

2. Plot the network using sna package.

3. Plot the network using igraph, color by features.

4. Use the Walktrap Community Method on the data to find out the clusters, including a dendrogram plot and a walkstrap community cluster plot.

5. Use sna package to compute parameters such as centrality values and plot the distributions of each plot.

6. Modeling facebook social network using StatNet package: MCMC diagnostics, goodness of fit plots, etc.


## Results:

From the igraph plot colored by attributes, we see that most of the users from the network is from the same location. 

According to MCMC diagnostics summary results, we can see that education degree and birthday are significantly asociated with the network in a positive direction, and language is significantly associated in a negative direction.
