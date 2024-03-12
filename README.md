# NLP-QA-BENCHMARK
This page contains the evaluation collection for Question Answering over CIDOC-CRM on Smithsonian American Art Museum (SAAM) KB. 
## Evaluation Set
The main evaluation set is made by 25 different questions on Smithsonian American Art Museum (SAAM) Knowledge Base. The directory include the manual comparison dataset (evaluation_dataset.json), used to evaluate the results, and the results of the process (evaluation_dataset_results.csv).
## CIDOC-QA-BENCHMARK Set

The second evaluation is about a set of 5'000 questions based on 10 query templates (of different radius). These questions are retrieved from 
N. GOUNAKIS, M. MOUNTANTONAKIS, and Y. TZITZIKAS "Evaluating a Radius-based Pipeline for Question Answering over Cultural
(CIDOC-CRM based) Knowledge Graphs" (2023). The datasets can be found at this link https://github.com/NicolaiGoon/CIDOC-QA-BENCHMARK/. The directory CIDOC-QA-BENCHMARK includes the results grouped by template, with 500 samples for each one. 

### Templates
In the CIDOC-QA-BENCHMARK repository, can be found the SPARQL template queries that sent to <https://triplydb.com/smithsonian/american-art-museum/sparql/american-art-museum>. The placeholders {Art Work} and {Artist} are replaced with entities present in the KG. From original templates, we add the following filtrer on SPARQL Query to specify the label of Art Work or Artist).

Case {Art Work}

```sparql
[...]
Filter(REGEX(str(?label), "{Art Work}","i")).
[...]
```

Case {Artist}

```sparql
[...]
Filter(REGEX(str(?label), "{Artist}","i")).
[...]
```
