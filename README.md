# NLP-QA-BENCHMARK
This page contains the evaluation collection for Question Answering over CIDOC-CRM on Smithsonian American Art Museum (SAAM) KG. In particular 5'000 questions are provided based on 10 query templates (of different radius). These questions are retrieved from 
N. GOUNAKIS, M. MOUNTANTONAKIS, and Y. TZITZIKAS "Evaluating a Radius-based Pipeline for Question Answering over Cultural
(CIDOC-CRM based) Knowledge Graphs" (2023). The datasets can be found at this link https://github.com/NicolaiGoon/CIDOC-QA-BENCHMARK/

## Templates
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


## Results
### Validation
The validation set is made by 25 different questions on mithsonian American Art Museum (SAAM) KG. The directory include the manual comparison dataset (vs_manual.json), used to evaluate the results, and the results of the process (vs_results.csv). The directory includes the results of the process using a Named Entity Recognition system (vs_results_with_NER.csv), based on Gazetteers of art works and artists in SAAM KG. 
### CIDOC-QA
The directory includes the results on the process of system with the natural language set used in "Evaluating a Radius-based Pipeline for Question Answering over Cultural
(CIDOC-CRM based) Knowledge Graphs". The results are grouped by template, with 500 samples for each one. 
