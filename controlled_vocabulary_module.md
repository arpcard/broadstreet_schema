# Controlled Vocabulary Module

Ontologies provide the organizing principle for data in CARD and allow the power of [Extensional Mereology](https://en.wikipedia.org/wiki/Mereology) in searching of data. CARD's Controlled Vocabulary Module in an extension of the [Chado CV Module](http://gmod.org/wiki/Chado_CV_Module). Note that when curating CARD, "ontology module", "cvterm module", and "controlled vocabulary module" are all synonymous terms. This page describes the Broad Street version of the Chado ontology SQL schema, not the ontologies themselves. To review CARD's ontologies, see [Ontologies](/ontologies/).

A variety of metadata are associated with ontology terms in CARD, via the *cvtermprop*, *cvtermsynonym*, and *cvterm_dbxref* tables. To support association of publications with ontology terms, we added the custom *cvterm_pub* table and similarly we added the *cvterm_models* table to link ontology terms to the [Model Module](model_module.md).

The [External Reference Module](dbxref_module.md) has a good explanation of how [Orthogonality](/ontologies/orthogonality.md) can be supported in Broad Street, e.g. placement of a Gene Ontology term/accession within the Antibiotic Resistance Ontology.

## *cv* table

The cv table defines each ontology used by Broad Street, whether developed internally or imported from the [OBO Foundry](https://obofoundry.org).

| Field | Description |
|-------|-------------|
| cv_id | unique identifier for each ontology |
| name | name of the ontology |
| definition | description of the ontology (often NULL) |

### Frequently used Ontologies

| cv_id | database |
|-------|-------------|
| 23 | [NCBI Taxonomy Ontology](/ontologies/ncbi_taxonomy_ontology.md) |
| 24 | [Antibiotic Resistance Ontology](/ontologies/antibiotic_resistance_ontology.md) |
| 25 | [Relationship Ontology](ontologies/relationship_ontology.md) |
| 26 | [Model Ontology](model_ontology.md) |

## *cvterm* table

The *cvterm* table defines each ontology term, e.g. [beta-lactamase](https://dune.mcmaster.ca/ontology/36010).

| Field | Description |
|-------|-------------|
| cvterm_id | unique identifier for each ontology term |
| cv_id | ontology to which the term belongs |
| name | name of the ontology term |
| definition | description of the ontology term |
| dbxref_id | the ontology accession via the [External Reference Module](dbxref_module.md) |
| is_obsolete | flag for term status, 0 = in use, 1 = obsolete |
| is_relationshiptype | for term type, 0 = descriptive term, 1 = relationship term |
| name_tsvector | legacy search index (depreciated) |
| definition_tsvector | legacy search index (depreciated) |
| cvterm_class | [Classification Tag](/ontologies/classification_tags.md) (if applicable) |

## *cvtermsynonym* table

The *cvtermsynonym* table records synonyms for the *cvterm.name* values as often genes have multiple name or antibiotics have chemical and trade names.

| Field | Description |
|-------|-------------|
| cvtermsynonym_id | unique identifier for each synonym |
| cvterm_id | the ontology term being connected to the synonym |
| synonym |  text of the synonym |
| type_id | type of synonym as defined by *cvterm_id* in the [Synonym Ontology](/ontologies/synonym_ontology.md) |
| synonym_tsvector | legacy search index (depreciated) |

Note, we currently use *cvterm_id*=35 (exact) as *type_id* in Broad Street, but do not make type visible on the website.

## *cvtermprop* table

The *cvtermprop* table records properties for an ontology term, a very Chado-like approach for storing features associated with a term as opposed to external references (publications, structures, models). We use *cvtermprop* to store curator Notes (*cvterm_id*=35430 in the [Publication Ontology](ontologies/pub_ontology.md)) and [CARD Short Names](/ontologies/card_short_names.md) (*cvterm_id*=45489 in the [Antibiotic Resistance Ontology](ontologies/antibiotic_resistance_ontology.md)).

| Field | Description |
|-------|-------------|
| cvtermprop_id | unique identifier for each property |
| cvterm_id | the ontology term being connected to the property |
| type_id | the type of property, i.e. Note from the [Publication Ontology](ontologies/pub_ontology.md) or CARD Short Name from the [Antibiotic Resistance Ontology](/ontologies/antibiotic_resistance_ontology.md) |
| value | value of the property, e.g. the text of the curator Note |
| rank | rank of the property, for sorting or order of presentation (not used) |

## *cvterm_relationship* table

The cvterm_relationship table defines the relationships between ontology terms, e.g. part_of, is_a, etc.

| Field | Description |
|-------|-------------|
| cvterm_relationship_id | unique identifier for each relationship |
| type_id | type of relationship defined by a *cvterm_id* from the [Relationship Ontology](ontologies/relationship_ontology.md) |
| subject_id | child node *cvterm_id* |
| object_id | parent node *cvterm_id* |

For example: 

* [TEM-13](https://dune.mcmaster.ca/ontology/37265) (subject_id) [is_a](https://dune.mcmaster.ca/ontology/31) (type_id) [TEM beta-lactamase](https://dune.mcmaster.ca/ontology/36023) (object_id)
* [TEM beta-lactamase](https://dune.mcmaster.ca/ontology/36023) (subject_id) [confers_resistance_to_drug_class](https://dune.mcmaster.ca/ontology/36382) (type_id) [monobactam](https://dune.mcmaster.ca/ontology/35923) (object_id)

![TEM-13](/img/TEM-13.jpg)

## *cvtermpath* table (a [materialization](https://en.wikipedia.org/wiki/Materialized_view))

Key to many algorithmic calculations in CARD are the contents of the *cvtermpath* table, which is used to store calculated closure for ontologies, i.e. length (d) of all paths between two terms within an ontology. While the other tables in the Controlled Vocabulary Module are curated, the contents of *cvtermpath* must be calculated after any change to the contents of the *cvterm_relationship* table. We are not using the scripts provided by GMOD as we found errors with the output, we were not interested in all the output, and they were generally very slow. We developed our own script to fill the *cvtermpath* table from the contents of the *cvterm_relationship* table, see [Automated Tasks](automated_tasks.md). This software calculates direct (d=1) and [transitive](https://en.wikipedia.org/wiki/Transitive_closure) (d>1) but not [reflexive](https://en.wikipedia.org/wiki/Reflexive_closure) (d=0) closure for ontologies but stores only positive transitive closure values in the cvtermpath table (to reduce size). CARD thus only uses positive values when calculating the *cvtermpath* table. The software calculates transitive closure values regardless of relationship type between ontology terms (is_a, part_of, regulated_by, etc.), i.e. closure is not deductive. However, we have added the custom field *cvtermpath.exclusive_type_id*. If the entry in *cvtermpath* involves only one *type_id* throughout the entire path in question, the value of that *type_id* will be placed in *cvtermpath.exclusive_type_id*. Otherwise, it will be NULL. This is useful for looking for paths within branches of the ARO as within-branch paths are almost exclusively is_a.

| Field | Description |
|-------|-------------|
| cvtermpath_id | unique identifier for path between two ontology terms |
| type_id | the relationship type that this is a closure over (not used, see above) |
| subject_id | node *cvterm_id* at start of path, e.g. TEM-13 |
| object_id | node *cvterm_id* at end of path, e.g. beta-lactamase |
| cv_id | ontology containing the subject_id and object_id ontolgy terms |
| pathdistance | the number of relationships edges among nodes in the ontology path |
| exclusive_type_id | if path contains only one relationship type, e.g. is_a, it is indicated, otherwise the value is NULL |
| completepath | dash-delimited cvterm_id of the entire path, from object_id to subject_id, e.g. 36010-36217-36023-37265 |
| relationships | entire path in a text format, e.g. {36217-36023:31,36023-37265:31,36010-36217:31} |

## *cvterm_dbxref* table

The *cvterm_dbxref* table connects ontology terms to external related data, such as antibiotic or protein structures in PubChem or PDB, via the [External Reference Module](database_modules/dbxref_module.md):

| Field | Description |
|-------|-------------|
| cvterm_dbxref_id | unique identifier for each cvterm dbxref join |
| cvterm_id | associated cvterm identifier found in the *cvterm* table |
| dbxref_id | associated PubChem or PDB accession in the [External Reference Module](database_modules/dbxref_module.md) |
| is_for_definition | Chado legacy provenance tracking (depreciated) |

## *cvterm_models* joins table

Ontology terms are associated with AMR detection models in the [Model Module](database_modules/model_module.md) via the following joins table:

| Field | Description |
|-------|-------------|
| cvterm_models_id | unique identifier for each cvterm models join |
| cvterm_id | associated cvterm identifier found in the *cvterm* table |
| model_id | associated model identifier found in the [Model Module](database_modules/model_module.md) |

## *cvterm_pub* joins table

Ontology terms are associated with publications in the [Publication Module](database_modules/publication_module.md) via the following joins table:

| Field | Description |
|-------|-------------|
| cvterm_pub_id | unique identifier for each cvterm pub join |
| cvterm_id | associated cvterm identifier found in the *cvterm* table  |
| pub_id | associated publication identifier found in the [Publication Module](database_modules/publication_module.md) |

## *replaced* table

At the CARD curation interface, there is an option to replace an existing ontology term with another existing ontology term, tracked by *replaced*:

| Field | Description |
|-------|-------------|
| replaced_id | unique identifier for each replacement |
| cvterm_id | cvterm_id of ontology term being replaced |
| replaced_by | cvterm_id of ontology term doing the replacement |
| created_at | timestamp |

