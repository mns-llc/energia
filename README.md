# energia[*](https://www.youtube.com/watch?v=BVWfqOSdzs4)

For some of our research, we've discovered many JSONified nested arrays (ElasticSearch results, cough cough). In order to more readily interpret the data, we needed a way to compute relative complexities of the nested array structure - signatures, complexity, and so forth. The logic behind this being:
1. There are probably many known datasets with a predictable signature that we would like to deprioritize ex. example datasets, generic logs, or so forth.
2. Several of the ElasticSearch results could have been discovered by simply looking for complex nested arrays, ex. deep, inconsistent, broad structure.

## Signatures
Stil ruminating.

## Complexity
Partially inspired by Kolmogorov complexity, we implement a four-metric scoring system for complexity which should allow us to distill how complex certain document structures in ElasticSearch results are:

**Full structural complexity:**
* Start a counter at 0
* For each value that maps to another array, add 1
* For all other values, add 0.1

**Cleaned data structural complexity:**
* Remove any key->value pairs that are duplicated (removing both the original and all duplicates)
* Recompute structural complexity

**Cleaned key structural complexity:**
* For any value that is not mapping to another array, null the value
* Remove any key->value pairs that are duplicated elsewhere (removing both the original and all duplicates), ignoring key->array mappings
* Recompute structural complexity

**Skeleton structural complexity:**
* Flatten nested array into one exceptionally long array
* For any keys that should have mapped to an array, recreate them as null
* Remove any key->value pairs that are duplicated elsewhere (removing both the original and all duplicates)
* Count the number of keys remaining and multiply by 0.1
