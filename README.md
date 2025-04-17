# Corner Generative Benchmarking Demo

We can apply [generative benchmarking](https://research.trychroma.com/generative-benchmarking) to evaluate embedding models that power retrieval for Corner's vibe search. Rather than trusting industry benchmark datasets which can be generic, overly clean, or already seen in training, we can synthetically generate queries which are more representative of our real world production data.

## 1. Dataset Curation
For this demo, I sourced customer review data from the [Yelp Open Dataset](https://business.yelp.com/data/resources/open-dataset/). I extracted reviews for local cafes and took a sample of 1000 data points, large enough to be representative of the distribution.

## 2. Query Generation

### Filter Documents
We provide context and filtering criteria to an aligned LLM judge to identify documents that are most relevant to our retrieval use case and contains sufficient information to generate queries from.

### Generate Queries
We generate queries using gpt-4o the given context and example queries to steer the generation. This golden dataset will be commonly used to evaluate different embedding models.
