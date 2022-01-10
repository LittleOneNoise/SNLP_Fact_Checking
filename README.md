# SNLP Fact Checking
A corpus-driven fact-checking engine, which returns a confidence value between 0 (fact is false) and 1 (fact is true) given a fact

## Approach/Data preparation
We used the train dataset in order to build the main skeleton of the engine. In order to do this, we retrieved all the information from the given raw sentence and collected related information in dictionary of dictionaries (corresponding to the each fact).
As a result we ended up with the following structure for a sentence that includes the main information that is used for the fact checking.<br/><br/>
Since there are **three types** of sentence structure we handled them with different scenarios:<br/><br/>
`1. Sentence: Albert Einstein's honour is Nobel Prize in Physics.`<br/>
> -> Main information list : ['Albert Einstein', 'Nobel Prize in Physics'], category: honour, synonym of category that exists in other facts as a category: award

`2. Sentence: Nobel Prize in Physics is Albert Einstein's award.`
> -> Main information list: ['Nobel Prize in Physics', 'Albert Einstein'], category: award, synonym of category that exists in other facts as a category: honour

`3. Sentence: Stars Nokia, Finland has been Nokiaed Nokia.`
> -> Main information list: ['None', 'None'], category: stars, synonym does not exist

???_Notice that in the information that was provided above not all information that each dictionary consist???_. The other information such as raw_sentence, splitted_sentence and the others were used for different purposes of building the skeleton. 
In order to explain the skeleton in detail, some important notes related with sentence structures must be given.

Each sentence includes 2 different facts that we call specific fact and generic fact. In example 1 and 2 (given above) Albert Einstein is the specific information while Nobel Prize in Physics stands for generic information.
This priority was used when we built the local database. If you pay attention you will see that sentence structures differ with respect to the position of 'category'. We used this knowledge as a key to prioritize the sentences like in the 2nd example, since the sentences look like the 1st one were already prioritized by the structure. Additionally the sentences like the 3rd example were labelled as False directly, since they do not follow the main structures of sentences that leads the engine to confuse. After prioritization application, result will be:

`2. Sentence: Nobel Prize in Physics is Albert Einstein's award.`
> -> Main information list: ['Albert Einstein', 'Nobel Prize in Physics'], category: award, synonym of category that exists in other facts as a category: honour
