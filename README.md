# NLP-QA-BENCHMARK
This page contains the evaluation collection for Question Answering over CIDOC-CRM on Smithsonian American Art Museum (SAAM) KG. In particular 5'000 questions are provided based on 10 query templates (of different radius). These questions are retrieved from 
N. GOUNAKIS, M. MOUNTANTONAKIS, and Y. TZITZIKAS "Evaluating a Radius-based Pipeline for Question Answering over Cultural
(CIDOC-CRM based) Knowledge Graphs" (2023). The datasets can be found at this link https://github.com/NicolaiGoon/CIDOC-QA-BENCHMARK/

## Templates
Below, we provide the different SPARQL Template queries that sent to <https://triplydb.com/smithsonian/american-art-museum/sparql/american-art-museum>. The placeholders {Art Work} and {Artist} are replaced with entities present in the KG. From queries in Gounakis23, we add the following filtrer on SPARQL Query to specify the label of Art Work or Artist).

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

The results are stored in the folder "results". There can be found a file for each template and a file with a sample of 10 elements for each template.
