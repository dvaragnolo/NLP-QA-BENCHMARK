# NLP-QA-BENCHMARK
This page contains the evaluation collection for Question Answering over CIDOC-CRM on Smithsonian American Art Museum (SAAM) KG. In particular 50000 questions are provided based on 10 query templates (of different radius). These questions are retrieved from 
N. GOUNAKIS, M. MOUNTANTONAKIS, and Y. TZITZIKAS "Evaluating a Radius-based Pipeline for Question Answering over Cultural
(CIDOC-CRM based) Knowledge Graphs" (2023).

## Templates
Below, we provide the different SPARQL Template queries that we sent to <https://triplydb.com/smithsonian/american-art-museum/sparql/american-art-museum>. The placeholders {Art Work} and {Artist} are replaced with entities present in the KG. 

Q1. Which is the art type of {Art Work}?

```sparql
SELECT ?artwork ?label ?typeLabel WHERE { 
?artwork rdfs:label ?label . 
?artwork cidoc:P2_has_type ?type .  
?type rdfs:label ?typeLabel .
Filter(REGEX(str(?label), "{Art Work}","i")).
} 
```

Q2. What material used for creating the {Art Work}?

```sparql
SELECT ?artwork ?label  ?material WHERE { 
?artwork rdfs:label ?label .  
?artwork cidoc:P67i_is_referred_to_by ?ref . 
?ref rdf:value ?material 
.filter(STRENDS(str(?ref),'medium')) .
Filter(REGEX(str(?label), "{Art Work}","i")). 
} 
```

Q3. Which gave the {Art Work} to the museum?

```sparql
SELECT ?artwork ?label ?value WHERE { 
?artwork rdfs:label ?label . 
?artwork cidoc:P67i_is_referred_to_by ?ref .  
?ref rdf:value ?value 
.FILTER(REGEX(STR(?value) , "Gift of" ) || REGEX(STR(?value) , "Bequest of" )) .
Filter(REGEX(str(?label), "{Art Work}","i")).  
} 
```

Q4. Who is the creator of {Art Work}?

```sparql
SELECT ?artwork ?label ?value WHERE {  
?artwork rdfs:label ?label .  
?artwork cidoc:P108i_was_produced_by ?prod .   
?prod cidoc:P14_carried_out_by ?actor .  
?actor rdfs:label ?value .
Filter(REGEX(str(?label), "{Art Work}","i")).  
} 
```

Q5. Which is the birth place of {Artist}?

```sparql
SELECT distinct ?actor ?label ?placeLabel WHERE { 
?actor rdfs:label ?label .  
?actor cidoc:P92i_was_brought_into_existence_by ?existence .  
?existence cidoc:P7_took_place_at ?place . 
?place rdfs:label ?placeLabel .
Filter(REGEX(str(?label), "{Artist}","i")). 
} 
```

Q6. When the production of {Art Work} started?

```sparql
SELECT ?artwork ?label ?date WHERE { 
?artwork rdfs:label ?label . 
?artwork cidoc:P108i_was_produced_by ?prod .  
?prod cidoc:P4_has_time-span ?tsp .  
?tsp cidoc:P82a_begin_of_the_begin ?date .
Filter(REGEX(str(?label), "{Art Work}","i")). 
} 
```

Q7. When the production of {Art Work} ended?

```sparql
SELECT ?artwork ?label ?endDate WHERE { 
?artwork rdfs:label ?label . 
?artwork cidoc:P108i_was_produced_by ?prod .  
?prod cidoc:P4_has_time-span ?tsp .  
?tsp cidoc:P82b_end_of_the_end ?endDate .
Filter(REGEX(str(?label), "{Art Work}","i")). 
}  
```

Q8. Which is the nationality of the creator of {Art Work}?

```sparql
SELECT ?artwork ?label ?nationality WHERE { 
?artwork rdfs:label ?label .   
?artwork cidoc:P108i_was_produced_by ?prod .    
?prod cidoc:P14_carried_out_by ?actor .    
?actor cidoc:P107i_is_current_or_former_member_of ?country .    
?country rdfs:label ?nationality .
Filter(REGEX(str(?label), "{Art Work}","i")).
}  
```

Q9. Which is the birth place of the creator of {Art Work}?

```sparql
SELECT ?artwork ?label ?placeLabel WHERE { 
?artwork rdfs:label ?label . 
?artwork cidoc:P108i_was_produced_by ?prod .  
?prod cidoc:P14_carried_out_by ?actor .  
?actor cidoc:P92i_was_brought_into_existence_by ?existence .  
?existence cidoc:P7_took_place_at ?palce . 
?place rdfs:label ?placeLabel .
Filter(REGEX(str(?label), "{Art Work}","i")). 
} 
```

Q10. Which year died the creator of {Art Work}?

```sparql
SELECT ?artwork ?label ?deathYear WHERE { 
?artwork rdfs:label ?label . 
?artwork cidoc:P108i_was_produced_by ?prod .  
?prod cidoc:P14_carried_out_by ?actor .  
?actor cidoc:P93i_was_taken_out_of_existence_by ?out .    
?out cidoc:P4_has_time-span ?ts .   
?ts rdfs:label ?deathYear .
Filter(REGEX(str(?label), "{Art Work}","i")).
} 
```

## Results

The results are stored in the folder "results". There can be found a file for each template and a file with a sample of 10 elements for each template.
