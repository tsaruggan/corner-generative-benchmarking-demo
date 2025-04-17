# Corner Generative Benchmarking Demo

We can apply [generative benchmarking](https://research.trychroma.com/generative-benchmarking) to evaluate embedding models that power retrieval for Corner's vibe search. Rather than trusting industry benchmark datasets which can be generic, overly clean, or already seen in training, we can synthetically generate queries which are more representative of our real world production data.

## 1. Dataset Curation
For this demo, I sourced customer review data from the [Yelp Open Dataset](https://business.yelp.com/data/resources/open-dataset/). I extracted reviews for local cafes and took a sample of 1000 data points, large enough to be representative of the distribution.

## 2. Query Generation

### Filter Documents
We provide context and filtering criteria to an aligned LLM judge to identify documents that are most relevant to our retrieval use case and contains sufficient information to generate queries from.

### Generate Queries
We generate queries using gpt-4o the given context and example queries to steer the generation. This golden dataset will be commonly used to evaluate different embedding models.

| cafe                               | address                                  | review                                                                                                  | generated query                                 |
|:-----------------------------------|:-----------------------------------------|:--------------------------------------------------------------------------------------------------------|:------------------------------------------------|
| Willa Jean                         | 611 O'Keefe Ave, New Orleans, LA         | Hello lover! This restaurant was my inspiration for finally starting this account! I had to get the ... | best chocolate croissant in town                |
| Bodega                             | 1180 Central Ave, Saint Petersburg, FL   | Fidel would be proud. This place is the jam. I always order the Lechon when I come here and everytim... | authentic Cuban cuisine with specials           |
| Cafe Patachou                      | 225 W Washington St, Indianapolis, IN    | Brunch recommendation and it didn't disappoint! Went at about 10 am and there wasn't any waiting on ... | vegan brunch with unique flavors                |
| First Watch                        | 985 NW Plaza Dr, St.Ann, MO              | Yum!!! This place definitely delivers. From its clean decor and tastefully staged tables to the frie... | avocado toast with sides                        |
| Cartel Roasting                    | 2516 N Campbell Ave, Tucson, AZ          | Me and my husband stopped at Cartel while going shopping to Tucson Mall. We were really stressed and... | warm and inviting atmosphere                    |
| Doormét                            | 1155 S Dale Mabry Hwy, Ste 12, Tampa, FL | We go here all the time during lunch. It is know for gourmet food delivered to your door but if you ... | fresh ingredients and great service             |
| BrightSide Artisan Comfort Cafe    | 204 N Morgan St, Tampa, FL               | All I had is their pre-made dinner, chicken Florentine (eggplant is succulent), Mac and cheese (that... | casual dinner with comfort food                 |
| Roots Cafe                         | 133 E Gay St, West Chester, PA           | This place reminds me of a cute little tree house. ORGANIC. FARM-TO-TABLE. RUSTIC. BRUNCH. COCKTAILS... | rustic brunch with organic options              |
| Cafe Patachou                      | 5790 E Main St, Carmel, IN               | Located off of Hazel Dell and 131st/Main Street, Cafe Patachou is in the same shopping area as Pizza... | lively cafe with patio seating                  |
| Hanjan                             | 3735 99 Street NW, Edmonton, AB          | I went to Hanjan for a friends birthday. I've been here many times and it never disappoints! Hanjan ... | romantic spot with cozy vibes                   |
| Blue Willow Restaurant & Gift Shop | 2616 N Campbell Ave, Tucson, AZ          | "Bye Felicia!" is what one of their cute dish towels had written on it. Their gift store is adorable... | family-friendly cafe with good customer service |
| Smyrna Cafe                        | 1666 Lee Victory Pkwy, Smyrna, TN        | After Sebastians went out of business, my wife and I had been looking for a great local restaurant t... | great Cajun pasta spot                          |
| Ruby Slipper - New Orleans         | 200 Magazine St, New Orleans, LA         | BEST 23rd birthday brunch I could have asked for! 8 of my best friends, eggs Benny and an order of b... | birthday brunch with friends                    |

## 3. Benchmark Experiments
We setup different embedding model providers with Chroma's vector database and then run benchmark evaluations with our golden dataset of queries to gain powerful metrics. Performance is measured over the top k retrieved results:
![benchmark results](/img/benchmark_results.png)

- Recall: Proportion of relevant documents retrieved out of all relevant ones.  
- Precision: Proportion of retrieved documents that are relevant.  
- NDCG: Measures ranking quality by rewarding relevant documents appearing earlier.  
- MAP: Mean average precision across queries, considering both relevance and ranking.

## 4. Compare Results
We can visualize the results and assess relevant metrics that are important for Corner users to make an informed decision about which model provider to use.
<div>
  <img src="/img/recall_3.png" width="400px" />
  <p>Recall@3: Measures how often a relevant result appears in the top 3.</p>
</div>

<div>
  <img src="/img/ndcg_5.png" width="400px" />
  <p>NDCG@5: Rewards relevant documents ranked higher in the top 5.</p>
</div>

<div>
  <img src="/img/precision_10.png" width="400px" />
  <p>Precision@10: Proportion of relevant results in the top 10.</p>
</div>

