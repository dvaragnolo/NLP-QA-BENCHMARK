# Authors

Davide Varagnolo (_d.varagnolo@studenti.unipi.i_), Department of Informatics, University of Évora, Portugal and NOVA Laboratory for Computer Science and Informatics, NOVA LINCS, Portugal

Dora Melo (_dmelo@iscac.pt_), Coimbra Business School | ISCAC, Polytechnic Institute of Coimbra, Portugal, CEOS.PP Coimbra, Polytechnic Institute of Coimbra, Portugal, and NOVA Laboratory for Computer Science and Informatics, NOVA LINCS, Portugal

Irene Pimenta Rodrigues (_ipr@uevora.pt_), Department of Informatics, University of Évora, Portugal and NOVA Laboratory for Computer Science and Informatics, NOVA LINCS, Portugal

# Introduction

This page presents a performance evaluation of a Question-Answering System that automatically translates Natural Language Questions into SPARQL queries, over the Smithsonian American Art Museum’s (SAAM) CIDOC-CRM representation. 

The architecture of the system includes a pipeline with three modules: 
* Partial Semantic Representation, composed of two steps: the dependency parser of Stanza, and transformation into a set of partial Discourse Representation Structures (DRSs).
* Pragmatic interpretation, which rewrites the partial semantic representations of a question into a set of variant meanings of the question in the domain-specific context.
* SPARQL Generator which is applied to the Semantic Query Representation solution and generates the corresponding SPARQL query representation.

The repository is organized as follows: 
* SAAMs-50NLQuestions - A 50 natural language questions dataset manually defined and the corresponding SAAM's answers obtained from the SPARQL endpoint (https://triplydb.com/smithsonian/american-art-museum/sparql). Additionally, it includes, for each question, an evaluation of the automatic answer, the partial discourse representation, the solution DRS, and the generated SPARQL query.
* NicolaiGoon-CIDOC-QA-BENCHMARK - The evaluation results of 5000 questions, grouped into 10 different types of questions (each group with 500 questions). The system performance is evaluated both with NER and without NER to recognize SAAM's entity names. This question dataset was originally built by Nikos Gounakis, Michalis Mountantonakis, and Yannis Tzitzikas (2023) "Evaluating a Radius-based Pipeline for Question Answering over
Cultural (CIDOC-CRM based) Knowledge Graphs" (in Proceedings of the 34th ACM Conference on Hypertext and Social Media, Rome, Italy, HT ’23, Association for Computing Machinery, New York, NY, USA, Article 24, https://doi.org/10.1145/3603163.3609067), and made available from https://github.com/NicolaiGoon/CIDOC-QA-BENCHMARK/.

## SAAMs-50NLQuestions
The 50 English questions are on artists and artefacts in the Smithsonian American Art Museum (SAAM) Knowledge Base.

 
| --- |
| Who is Darryl Abraham? |
| What is Honeymoon Motel? |
| Which paintings did Leonard Ochtman create? |
| What was the birthdate of Leonard Ochtman? |
| What was the birthplace of Leonard Ochtman? |
| What was the deathdate of Leonard Ochtman? |
| When did Leonard Ochtman die? |
| When died Leonard Ochtman? |
| Where was the deathplace of Leonard Ochtman? |
| What was the nationality of Leonard Ochtman? |
| Who painted the Morning Haze? |
| Who donated the Morning Haze? |
| Who gave the Morning Haze? |
| When was the production of Morning Haze completed? |
| When was the Morning Haze completed? |
| When did the production of Morning Haze start? |
| When did Morning Haze start? |
| Who is the author of Morning Haze? |
| When was the Morning Haze created? |
| What was the birthplace of the creator of the Morning Haze? |
| What is the size of the Morning Haze? |
| Who gave the Honeymoon Motel to the museum? |
| What are the sculptures that are made of lead? |
| Who was born in 1854? |
| Who was born in Zonnemaire? |
| Who are the authors of the Morning Haze? |
| Which are the authors of Morning Haze? |
| What is the place where Leonard Ochtman was born? |
| What sculptures are made of lead? |
| What sculptures are made of lead material? |
| Who made artworks with lead and wood? |
| Who made Morning Haze and Summer Morning? |
| Which painters have died in Amsterdam? |
| What things are made by the guy Leonard Ochtman? |
| What artefacts are made of lead? |
| What arefacts are made by the artist Leonard Ochtman? |
| When did the production of Morning Haze end? |
| What authors died in Madrid in 1660? |
| When did the creation of Morning Haze start? |
| When was the production of Morning Haze ended? |
| When did Leonard Ochtman paint Morning Haze? |
| Where did the painter of Morning Haze born? |
| Which painter died at the birthplace of Leonard Ochtman? |
| Who gave sculptures made by Leonard Ochtman? |
| What are the artefacts produced at the deathdate of Leonard Ochtman? |
| Who was born between 1950 and 1970? |
| Who was born before 1950? |
| Who was born in 1950 or in 1970? |
| What artefacts were made after 2000? |
| Where did Mary go in 1900? |

### Manual Validation Set 
For each question, the manual validation set is a JSON file which includes the NL question and the SPARQL response of the manually created SPARQL query that answers the natural language question, i.e. for the question "Which paintings did Leonard Ochtman create?" the corresponding JSON object is the following: 
```JSON
{
    "question": "Which paintings did Leonard Ochtman create?",
    "answers": [
      {
        "snameArtefact": "A Morning in Summer",
        "Artefact": "http://data.americanart.si.edu/object/id/1946.10.2",
        "snameArtist": "Leonard Ochtman",
        "Artist": "http://data.americanart.si.edu/constituent/id/3606"
      },
      {
        "snameArtefact": "Morning Haze",
        "Artefact": "http://data.americanart.si.edu/object/id/1909.11.2",
        "snameArtist": "Leonard Ochtman",
        "Artist": "http://data.americanart.si.edu/constituent/id/3606"
      }
    ]
  }
```
### System Results Set
| id | Question | Compare Results | Comment |
| -- | -------- | ----------------- | ------- |
| 1 | Who is Darryl Abraham? | True | ok |
| 2 | What is Honeymoon Motel? | True | ok |
| 3 | Which paintings did Leonard Ochtman create? | True | ok |
| 4 | What was the birthdate of Leonard Ochtman? | True | ok |
| 5 | What was the birthplace of Leonard Ochtman? | True | ok |
| 6 | What was the deathdate of Leonard Ochtman? | True | ok |
| 7 | When did Leonard Ochtman die? | True | ok |
| 8 | When died Leonard Ochtman? | True | ok |
| 9 | Where was the deathplace of Leonard Ochtman? | True | ok |
| 10 | What was the nationality of Leonard Ochtman? | True | ok |
| 11 | Who painted the Morning Haze? | True | ok |
| 12 | Who donated the Morning Haze? | True | ok |
| 13 | Who gave the Morning Haze? | True | ok |
| 14 | When was the production of Morning Haze completed? | True | ok |
| 15 | When was the Morning Haze completed? | True | ok |
| 16 | When did the production of Morning Haze start? | True | ok |
| 17 | When did Morning Haze start? | True | ok |
| 18 | Who is the author of Morning Haze? | True | ok |
| 19 | When was the Morning Haze created? | True | ok |
| 20 | What was the birthplace of the creator of the Morning Haze? | True | ok |
| 21 | What is the size of the Morning Haze? | True | ok |
| 22 | Who gave the Honeymoon Motel to the museum? | True | ok |
| 23 | What are the sculptures that are made of lead? | True | ok |
| 24 | Who was born in 1854? | True | ok |
| 25 | Who was born in Zonnemaire? | True | ok |
| 26 | Who are the authors of the Morning Haze? | True | ok |
| 27 | Which are the authors of Morning Haze? | True | ok |
| 28 | What is the place where Leonard Ochtman was born? | True | ok |
| 29 | What sculptures are made of lead? | True | ok |
| 30 | What sculptures are made of lead material? | False | Incorrect Query Ontology Representation |
| 31 | Who made artworks with lead and wood? | True | ok |
| 32 | Who made Morning Haze and Summer Morning? | False | Partial Query Ontology Representation |
| 33 | Which painters have died in Amsterdam? | True | ok |
| 34 | What things are made by the guy Leonard Ochtman? | True | ok |
| 35 | What artefacts are made of lead? | True | ok |
| 36 | What arefacts are made by the artist Leonard Ochtman? | True | ok |
| 37 | When did the production of Morning Haze end? | False | Incorrect Stanza interpretation |
| 38 | What authors died in Madrid in 1660? | True | ok |
| 39 | When did the creation of Morning Haze start? | True | ok |
| 40 | When was the production of Morning Haze ended? | True | ok |
| 41 | When did Leonard Ochtman paint Morning Haze? | True | ok |
| 42 | Where did the painter of Morning Haze born? | True | ok |
| 43 | Which painter died at the birthplace of Leonard Ochtman? | True | ok |
| 44 | Who gave sculptures made by Leonard Ochtman? | True | ok |
| 45 | What are the artefacts produced at the deathdate of Leonard Ochtman? | False | Incorrect Query Ontology Representation |
| 46 | Who was born between 1950 and 1970? | True | ok |
| 47 | Who was born before 1950? | True | ok |
| 48 | Who was born in 1950 or in 1970? | False | Partial Query Ontology Representation |
| 49 | What artefacts were made after 2000? | True | ok |
| 50 | Where did Mary go in 1900? | True | Incorrect Query Ontology Representation |

The file presents the results of all the sub-modules of the system: the Partial Semantic Representation, the Query Ontology Representation and the Genererated SPARQL Query. Moreover, are presented the SPARQL Response of the generated query, the SPARQL Response of the manual validation set and the comparation result between thw two responses. The comparation is boolean and it gives true if the SPARQL Response of the generated query contains the the SPARQL Response of the manual validation set.
Below, the result of the question "Which paintings did Leonard Ochtman create?":

```JSON
{
        "question": "Which paintings did Leonard Ochtman create?",
        "partialDRSs": " X1-create-create-null,X2-Leonard Ochtman-Leonard-null,X3-painting-paintings-which, X2-has_name-Leonard Ochtman,X1-subj-X2,X3-qualifier-which,X1-obj-X3,",
        "solution": {
            "dEntities": [
                "X1, create, create, VERB, null, false, Make1, Make",
                "X2, Leonard Ochtman, Leonard, PROPN, null, false, Artist2, Artist",
                "X3, painting, paintings, NOUN, which, false, Artefact3, Artefact",
                "X4, null, null, null, null, false, Query4, Query",
                "X5, null, null, null, null, false, Qualifier5, Qualifier"
            ],
            "dConditions": [
                "X2, has_name, Leonard Ochtman, properties= [] --- [has_name, has_text, who]",
                "X1, subj, X2, properties= [who_made] --- []",
                "X1, obj, X3, properties= [made_art] --- []",
                "X1, has_text, create, properties= [] --- [has_name, has_text, who]",
                "X2, has_text, Leonard Ochtman, properties= [] --- [has_name, has_text, who]",
                "X3, has_text, painting, properties= [] --- [has_name, has_text, who]",
                "X4, has_text, Query, properties= [] --- [has_name, has_text, who]",
                "X5, has_text, Qualifier, properties= [] --- [has_name, has_text, who]",
                "X5, has_name, which, properties= [] --- [has_name, has_text, who]",
                "X4, obj, X3, properties= [select] --- []",
                "X4, subj, X5, properties= [qQualifier] --- []"
            ],
            "score": {
                "numIndividuals": 5,
                "numEntitiesOk": 0,
                "numPropsOk": 16,
                "numEntitiesNOk": 0,
                "numClassesOk": 2
            }
        },
        "query": " PREFIX rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#> PREFIX xsd: <http://www.w3.org/2001/XMLSchema#> PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#> PREFIX cidoc: <http://www.cidoc-crm.org/cidoc-crm/> SELECT  DISTINCT  ?snameArtefact3 ?Artefact3 ?ltypeArtefact3 ?snameArtist2 ?Artist2 WHERE { ?Artefact3 cidoc:P2_has_type ?typeArtefact3.  ?typeArtefact3 rdfs:label ?ltypeArtefact3  . Filter(REGEX(?ltypeArtefact3, \"painting\",\"i\")). ?Artefact3 cidoc:P108i_was_produced_by ?Make1 . ?Artefact3 rdfs:label ?snameArtefact3 . ?Make1 cidoc:P14_carried_out_by ?Artist2 . ?Artist2 rdfs:label ?snameArtist2 . {{?Artist2 cidoc:P131_is_identified_by ?nameArtist2 . ?nameArtist2 rdf:value ?snameArtist2 .}  UNION {?Artist2 rdfs:label ?snameArtist2 .}}  filter(regex(?snameArtist2,\"Leonard Ochtman\",\"i\")) . }",
        "GenQueryResponse": [
            {
                "snameArtefact3": "Morning Haze",
                "Artefact3": "http://data.americanart.si.edu/object/id/1909.11.2",
                "ltypeArtefact3": "Painting",
                "snameArtist2": "Leonard Ochtman",
                "Artist2": "http://data.americanart.si.edu/constituent/id/3606"
            },
            {
                "snameArtefact3": "A Morning in Summer",
                "Artefact3": "http://data.americanart.si.edu/object/id/1946.10.2",
                "ltypeArtefact3": "Painting",
                "snameArtist2": "Leonard Ochtman",
                "Artist2": "http://data.americanart.si.edu/constituent/id/3606"
            }
        ],
        "ManQueryResponse": [
            {
                "Artist": "http://data.americanart.si.edu/constituent/id/3606",
                "snameArtist": "Leonard Ochtman",
                "Artefact": "http://data.americanart.si.edu/object/id/1946.10.2",
                "snameArtefact": "A Morning in Summer"
            },
            {
                "Artist": "http://data.americanart.si.edu/constituent/id/3606",
                "snameArtist": "Leonard Ochtman",
                "Artefact": "http://data.americanart.si.edu/object/id/1909.11.2",
                "snameArtefact": "Morning Haze"
            }
        ],
        "Gen.contains(Man)": "true"
    }

```


## CIDOC-QA-BENCHMARK Set - 5'000 NL Questions from https://github.com/NicolaiGoon/CIDOC-QA-BENCHMARK/

The second evaluation is about a set of 5'000 questions based on 10 query templates (of different radius). These questions are retrieved from 
N. GOUNAKIS, M. MOUNTANTONAKIS, and Y. TZITZIKAS "Evaluating a Radius-based Pipeline for Question Answering over Cultural
(CIDOC-CRM based) Knowledge Graphs" (2023). The datasets can be found at this link https://github.com/NicolaiGoon/CIDOC-QA-BENCHMARK/. The directory CIDOC-QA-BENCHMARK includes the results grouped by template, with 500 samples for each one. 

### Templates
In the CIDOC-QA-BENCHMARK repository, can be found the SPARQL template queries that sent to <https://triplydb.com/smithsonian/american-art-museum/sparql/american-art-museum>. The placeholders {Art Work} and {Artist} are replaced with entities present in the KG. From original templates, we add the following filtrer on SPARQL Query to specify the label of Art Work or Artist).
### Named Entity Recognition
Before the Syntactic Analysis performed by the parser Stanza, a module of Named Entity Recognition is applied. The NER is performed by a Gazetter's technique, which implies a pattern matching of a gazetteer containing all the art works' label and artists' name. The pattern matching is made by longest match. The entity recognized is substituted by a placeholder, recognized as proper name (PROPN) by Stanza. The placeholder is replaced by the original term on the final SPARQL Query generated. In the directory and in the summary below, the results are presented with the use NER module (NER_true) and without it (NER_false).

### Results
The results can be found on the directory CIDOC-QA-BENCHMARK. Each file is csv formatted. Files named "summary" show the results as the table previously presented in "System Result Set". Files named "results" show the results as the files from the evaluation set, without the columns "Gen Query Response" and "Man Data Response".

#### Q1 - Which is the art type of {Art Work}?

| N. Questions | % Correct Response with NER | % Correct Response without NER |
|--------------|-----------------------------|--------------------------------|
|      500     |             100.0%          |               44.0%            |

#### Q2 - What was the material used in the {Art Work}?

| N. Questions | % Correct Response with NER | % Correct Response without NER |
|--------------|-----------------------------|--------------------------------|
|      500     |             100.0%          |               44.8%            |

#### Q3 - Who gave the {Art Work} to the museum?

| N. Questions | % Correct Response with NER | % Correct Response without NER |
|--------------|-----------------------------|--------------------------------|
|      500     |             100.0%          |               50.4%            |

#### Q4 - Who is the creator of {Art Work}?

| N. Questions | % Correct Response with NER | % Correct Response without NER |
|--------------|-----------------------------|--------------------------------|
|      500     |             100.0%          |               55.6%            |

#### Q5 - Which is the birth place of {Artist}?

| N. Questions | % Correct Response with NER | % Correct Response without NER |
|--------------|-----------------------------|--------------------------------|
|      500     |             100.0%          |               98.0%            |

#### Q6 - When the production of {Art Work} started??

| N. Questions | % Correct Response with NER | % Correct Response without NER |
|--------------|-----------------------------|--------------------------------|
|      500     |             100.0%          |               49.4%            |

#### Q7 - When the production of {Art Work} ended?

| N. Questions | % Correct Response with NER | % Correct Response without NER |
|--------------|-----------------------------|--------------------------------|
|      500     |             100.0%          |               53.8%            |

#### Q8 - Which is the nationality of the creator of {Art Work}?

| N. Questions | % Correct Response with NER | % Correct Response without NER |
|--------------|-----------------------------|--------------------------------|
|      500     |             100.0%          |               46.4%            |

#### Q9 - Which is the birth place of the creator of {Art Work}?

| N. Questions | % Correct Response with NER | % Correct Response without NER |
|--------------|-----------------------------|--------------------------------|
|      500     |             100.0%          |               44.8%            |

#### Q10 - Which year died the creator of {Art Work}?

| N. Questions | % Correct Response with NER | % Correct Response without NER |
|--------------|-----------------------------|--------------------------------|
|      500     |             100.0%          |               56.0%            |





