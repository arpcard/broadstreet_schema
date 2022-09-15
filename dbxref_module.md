# External Reference Module

Broad Street's External Reference Module was adapted from [Chado](http://gmod.org/wiki/Introduction_to_Chado) for storing external references such as PMID, PDB accessions, GI numbers, etc. A number of other modules cross-reference to the *dbxref* table. Thus, when a part of Broad Street wishes to refer to an external accession, it references a *dbxref_id*. Using a centralized module for external references avoids duplicate information among modules and means a single correction immediately applies to all modules.

[[_TOC_]]

## Example

In the [Controlled Vocabulary Module](/database_modules/controlled_vocabulary_module.md), the *cvterm* table has the term for the [OSA-371](https://dune.mcmaster.ca/ontology/37957) gene with *cvterm.cvterm_id* 37957 and with *cvterm.dbxref_id* 331009. In the *dbxref* table, this *dbxref_id* has *dbxref.accession* 3001557 and *dbxref.db_id* 173. In the *db* table, this *db_id* is the ARO. Tracing the connections, within the Antibiotic Resistance Ontology OXA-371 is ARO:3001557.

![DBXREF](/img/cv_db.jpg)

Also in the *cvterm* table, the term for the OXA-371 gene is *cvterm.cv_id* 24. In the table *cv*, this *cv_id* is the Antibiotic Resistance Ontology, *cv.name* ARO. Why do we record this redundant information indicating that OXA-371 is in the ARO via *cv_id* when the accession is already ARO:3001557? See [Orthogonality](/ontologies/orthogonality.md).

## *db* table

This table is essentially a list of possible databases from which CARD may have information, with each given a unique identifier.

| Field | Description |
|-------|-------------|
| db_id | unique identifier for each external database |
| name | name of the database |
| description | description of the database (often NULL) |
| urlprefix | prefix for URL, e.g. PMID, but sometimes NULL |
| url | suffix for URL, e.g. ensemble.org (often NULL) |

### Frequently used Databases

| db_id | database |
|-------|-------------|
| 46 | PMID |
| 73 | DOI |
| 113 | PDB |
| 126 | PubChem |
| 180 | Protein Accession |
| 229 | DNA Accession |

## *dbxref* table

This table stores the individual external accessions. For example, the publication [CrpP Is Not a Fluoroquinolone-Inactivating Enzyme](https://pubmed.ncbi.nlm.nih.gov/34001507/) would have accession *db.name* via *dbxref.db_id* : *dbxref.accession*, i.e. PMID:34001507.

| Field | Description |
|-------|-------------|
| dbxref_id | unique identifier for each external accession |
| db_id | database for this accession |
| accession | the value of the accession |
| version | allows versioning (not used) |
| description | description of the accession (not used) |
| accession_tsvector | legacy search index (depreciated) |

