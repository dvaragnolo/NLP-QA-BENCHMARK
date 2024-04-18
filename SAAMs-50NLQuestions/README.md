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

## Named Entity Recognition (NER)
A Named Entity Recognition (NER) module is applied to recognize and tag the Artists and Artefacts within SAAM's knowledge base. 
The NER module uses a Gazetter containing all the artwork and artist names.

# SAAMs-50NLQuestions
The 50 English questions are on artists and artefacts in the Smithsonian American Art Museum (SAAM) Knowledge Base.

| nlp-qa50_questions.csv |
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
| What artefacts are made by the artist Leonard Ochtman? |
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

## SAAM's answers to the Natural Language Questions dataset
The SAAM's manually obtained answer to each Natural Language Question is represented in JSON format.
For instance, to the question "Which paintings did Leonard Ochtman create?" the complete information is presented as follows. 
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
## Evaluation of the Question-Answering System answer

The nlp-qa50_validation.json is organized as follows:
* Question = natural language question
* Compare Results = (Boolean Value) True when the system's answer equals the manual answer
* Comment = In case of failure, information on the system module with an incorrect result

| id | Question | Compare Results | Comment |
| -- | -------- | ----------------- | ------- |
| 1 | Who is Darryl Abraham? | True |  |
| 2 | What is Honeymoon Motel? | True |  |
| 3 | Which paintings did Leonard Ochtman create? | True |  |
| 4 | What was the birthdate of Leonard Ochtman? | True |  |
| 5 | What was the birthplace of Leonard Ochtman? | True |  |
| 6 | What was the deathdate of Leonard Ochtman? | True |  |
| 7 | When did Leonard Ochtman die? | True |  |
| 8 | When died Leonard Ochtman? | True |  |
| 9 | Where was the deathplace of Leonard Ochtman? | True |  |
| 10 | What was the nationality of Leonard Ochtman? | True |  |
| 11 | Who painted the Morning Haze? | True |  |
| 12 | Who donated the Morning Haze? | True |  |
| 13 | Who gave the Morning Haze? | True |  |
| 14 | When was the production of Morning Haze completed? | True |  |
| 15 | When was the Morning Haze completed? | True |  |
| 16 | When did the production of Morning Haze start? | True |  |
| 17 | When did Morning Haze start? | True |  |
| 18 | Who is the author of Morning Haze? | True |  |
| 19 | When was the Morning Haze created? | True |  |
| 20 | What was the birthplace of the creator of the Morning Haze? | True |  |
| 21 | What is the size of the Morning Haze? | True |  |
| 22 | Who gave the Honeymoon Motel to the museum? | True | 
| 23 | What are the sculptures that are made of lead? | True |  |
| 24 | Who was born in 1854? | True |  |
| 25 | Who was born in Zonnemaire? | True |  |
| 26 | Who are the authors of the Morning Haze? | True |  |
| 27 | Which are the authors of Morning Haze? | True |  |
| 28 | What is the place where Leonard Ochtman was born? | True |  |
| 29 | What sculptures are made of lead? | True |  |
| 30 | What sculptures are made of lead material? | False | Incorrect Query Ontology Representation |
| 31 | Who made artworks with lead and wood? | True |  |
| 32 | Who made Morning Haze and Summer Morning? | False | Partial Query Ontology Representation |
| 33 | Which painters have died in Amsterdam? | True |  |
| 34 | What things are made by the guy Leonard Ochtman? | True |  |
| 35 | What artefacts are made of lead? | True |  |
| 36 | What artefacts are made by the artist Leonard Ochtman? | True |  |
| 37 | When did the production of Morning Haze end? | False | Incorrect Stanza interpretation |
| 38 | What authors died in Madrid in 1660? | True |  |
| 39 | When did the creation of Morning Haze start? | True |  |
| 40 | When was the production of Morning Haze ended? | True |  |
| 41 | When did Leonard Ochtman paint Morning Haze? | True |  |
| 42 | Where did the painter of Morning Haze born? | True |  |
| 43 | Which painter died at the birthplace of Leonard Ochtman? | True |  |
| 44 | Who gave sculptures made by Leonard Ochtman? | True |  |
| 45 | What are the artefacts produced at the deathdate of Leonard Ochtman? | False | Incorrect Query Ontology Representation |
| 46 | Who was born between 1950 and 1970? | True |  |
| 47 | Who was born before 1950? | True |  |
| 48 | Who was born in 1950 or in 1970? | False | Partial Query Ontology Representation |
| 49 | What artefacts were made after 2000? | True |  |
| 50 | Where did Mary go in 1900? | False | Incorrect Query Ontology Representation |



The SAAMs-50q_results.json is organized as follows:
* question = natural language question
* partialDRSs = the question Partial Semantic Representation
* solution = the question Query-Ontology representation
* query = the generated SPARQL query of the question
* GenQueryResponse = the system SAAM's SPARQL endpoint answer to the query
* ManQueryResponse = the manual answer to the question
* Gen.contains(Man) = (Boolean Value) True when the system's answer equals the manual answer
  
For instance, to the question "Which paintings did Leonard Ochtman create?" the complete information is presented as follows. 

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
