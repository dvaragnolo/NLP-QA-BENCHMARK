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

# NicolaiGoon-CIDOC-QA-BENCHMARK

Additionally, the Question-Answering System performance is evaluated using a dataset of 5,000 questions over the SAAM's knowledge base.
The evaluation results of 5000 questions, were grouped into 10 types of questions (each group with 500 questions). The system performance is evaluated both with NER and without NER to recognize SAAM's entity names. This question dataset was originally built by Nikos Gounakis, Michalis Mountantonakis, and Yannis Tzitzikas (2023) "Evaluating a Radius-based Pipeline for Question Answering over Cultural (CIDOC-CRM based) Knowledge Graphs" (in Proceedings of the 34th ACM Conference on Hypertext and Social Media, Rome, Italy, HT ’23, Association for Computing Machinery, New York, NY, USA, Article 24, https://doi.org/10.1145/3603163.3609067), and made available from https://github.com/NicolaiGoon/CIDOC-QA-BENCHMARK/.

## Qi, i = 1..10
Each Qi, i=1..10, contains a type of question that is represented in the file Qi_results.json, and the system performance evaluation results with the use of a NER and without a NER, respective files  Qi_summary_NER_true.csv and Qi_summary_NER_false.csv, over 500 questions for each Qi.


### Qi_result.json, i=1..10

Each Qi_result.json is organized as follows:
* question = natural language question
* partialDRSs = the question Partial Semantic Representation
* solution = the question Query-Ontology representation
* query = the generated SPARQL query of the question

Below is an example of the question's type Q1, "Which is the art type of <Artwork>?".
```JSON
{
        "question": "Which is the art type of <Artwork>?",
        "partialDRSs": " X1-be-is-null,X2-which-Which-null,X4-<Artwork>-<Artwork>-a,X3-art type-art type-the, X1-subj-X2,X4-has_name-<Artwork>,X3-of-X4,X1-obj-X3, X1-be-is-null,X2-which-Which-null,X3-art type-art type-the,X4-<Artwork>-<Artwork>-a, X1-subj-X2,X1-obj-X3,X4-has_name-<Artwork>,X1-obl_of-X4,",
        "solution": {
            "dEntities": [
                "X1, be, is, VERB, null, false, Query1, Query",
                "X2, which, Which, PRON, null, false, Qualifier2, Qualifier",
                "X4, <Artwork>, <Artwork>, PROPN, a, false, Artefact3, Artefact",
                "X3, art type, art type, NOUN, the, false, ConceptArtefact4, ConceptArtefact"
            ],
            "dConditions": [
                "X1, subj, X2, properties= [qQualifier] --- []",
                "X4, has_name, <Artwork>, properties= [] --- [has_name, has_text, who]",
                "X3, of, X4, properties= [conc_of_Artefact] --- []",
                "X1, obj, X3, properties= [select] --- []",
                "X1, has_text, be, properties= [] --- [has_name, has_text, who]",
                "X2, has_text, which, properties= [] --- [has_name, has_text, who]",
                "X4, has_text, <Artwork>, properties= [] --- [has_name, has_text, who]",
                "X3, has_text, art type, properties= [] --- [has_name, has_text, who]",
                "X2, has_name, null list, properties= [] --- [has_name, has_text, who]",
                "X1, obj, X2, properties= [qQualifier] --- []"
            ],
            "score": {
                "numIndividuals": 4,
                "numEntitiesOk": 10,
                "numPropsOk": 1,
                "numEntitiesNOk": 0,
                "numClassesOk": 3
            }
        },
        "query": " PREFIX rdf: <http://www.w3.org/1999/02/22-rdf-syntax-ns#> PREFIX xsd: <http://www.w3.org/2001/XMLSchema#> PREFIX rdfs: <http://www.w3.org/2000/01/rdf-schema#> PREFIX cidoc: <http://www.cidoc-crm.org/cidoc-crm/> SELECT  DISTINCT  ?snameConceptArtefact4 ?ConceptArtefact4 ?snameArtefact3 ?Artefact3 WHERE { {{?Artefact3 cidoc:P131_is_identified_by ?nameArtefact3 . ?nameArtefact3 rdf:value ?snameArtefact3 .}  UNION {?Artefact3 rdfs:label ?snameArtefact3 .}}  filter(regex(?snameArtefact3,\"<Artwork>\",\"i\")) . ?Artefact3 cidoc:P2_has_type ?ConceptArtefact4. ?ConceptArtefact4 rdfs:label ?snameConceptArtefact4. }"
    }
```

### Qi_summary_NER_true.csv and Qi_summary_NER_false.csv, i=1..10

The files are organized as follows:
* Question = natural language question
* Gen.contains(Man) = (Boolean Value) True when the system's answer equals the manual answer
* Comment = In case of failure, information on the system module with an incorrect result


## Summary of the Evaluation Results
A summary of the Question-Answering System performance evaluation results obtained in each dataset Qi, i=1..10, is presented below.

### Q1 - Which is the art type of {Art Work}?

| N. Questions | % Correct Response with NER | % Correct Response without NER |
|--------------|-----------------------------|--------------------------------|
|      500     |             100.0%          |               44.0%            |

### Q2 - What was the material used in the {Art Work}?

| N. Questions | % Correct Response with NER | % Correct Response without NER |
|--------------|-----------------------------|--------------------------------|
|      500     |             100.0%          |               44.8%            |

### Q3 - Who gave the {Art Work} to the museum?

| N. Questions | % Correct Response with NER | % Correct Response without NER |
|--------------|-----------------------------|--------------------------------|
|      500     |             100.0%          |               50.4%            |

### Q4 - Who is the creator of {Art Work}?

| N. Questions | % Correct Response with NER | % Correct Response without NER |
|--------------|-----------------------------|--------------------------------|
|      500     |             100.0%          |               55.6%            |

### Q5 - Which is the birth place of {Artist}?

| N. Questions | % Correct Response with NER | % Correct Response without NER |
|--------------|-----------------------------|--------------------------------|
|      500     |             100.0%          |               98.0%            |

### Q6 - When the production of {Art Work} started?

| N. Questions | % Correct Response with NER | % Correct Response without NER |
|--------------|-----------------------------|--------------------------------|
|      500     |             100.0%          |               49.4%            |

### Q7 - When the production of {Art Work} ended?

| N. Questions | % Correct Response with NER | % Correct Response without NER |
|--------------|-----------------------------|--------------------------------|
|      500     |             100.0%          |               53.8%            |

### Q8 - Which is the nationality of the creator of {Art Work}?

| N. Questions | % Correct Response with NER | % Correct Response without NER |
|--------------|-----------------------------|--------------------------------|
|      500     |             100.0%          |               46.4%            |

### Q9 - Which is the birth place of the creator of {Art Work}?

| N. Questions | % Correct Response with NER | % Correct Response without NER |
|--------------|-----------------------------|--------------------------------|
|      500     |             100.0%          |               44.8%            |

### Q10 - Which year died the creator of {Art Work}?

| N. Questions | % Correct Response with NER | % Correct Response without NER |
|--------------|-----------------------------|--------------------------------|
|      500     |             100.0%          |               56.0%            |

