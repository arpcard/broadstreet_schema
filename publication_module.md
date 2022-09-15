# Publication Module

Broad Street's Publication Module was adapted from [Chado](http://gmod.org/wiki/Introduction_to_Chado) for storing all publication data, currently overwhelmingly based on PubMed identifiers (PMID). Publications in CARD are associated with ontology terms (*cvterm_pub* table in the [Controlled Vocabulary Module](/database_modules/controlled_vocabulary_module)) and model parameters (*model_parameters_pub* table in the [Model Module](/database_modules/model_module.md)). Publications are entered by curators using the web interface, after which CARD acquires all the data needed for the Publication Module via the NCBI API. In addition, as some publications are initially in press (i.e. epub ahead of print), the contents of the Publication Module are updated monthly as part of Broad Street [Automated Tasks](automated_tasks.md).

## Curating non-PMID publications

Broad Street, the CARD Website, and CARD Download files can support non-PMID publications by supporting DOIs. Although there is support for this on the curator interface, only the DOI is imported into Broad Street, leaving some fields empty for manual curation, see below. In these cases, the *dbxref_id* uses DOI (*db_id*=73) with the accession being the DOI text (e.g. *10.1039/C2MD20038D*), see https://devcard.mcmaster.ca:8888/root/card-website/-/issues/240. See [facT](https://dune.mcmaster.ca/ontology/37712) in CARD for a DOI-based publication.

## *pub* table

The *pub* table records information on each publication.

| Field | Description |
|-------|-------------|
| pub_id | unique identifier for each publication |
| title | publication title |
| volumetitle | title of volume is publication in part of a series (not used) |
| volume | journal volume |
| series_name | journal or series name |
| issue | journal issue |
| pyear | publication year |
| pages | publication pages |
| miniref | short reference, see below |
| uniquename | full reference, see below |
| type_id | the type of publication, defined in the [Publication Ontology](/ontologies/pub_ontology.md), but currently all publications are assigned cvterm_id 35432 (*paper*) |
| is_obsolete | flag for publication status, 0 = in use, 1 = obsolete (not used) |
| publisher | publisher information (not used) |
| pubplace | publisher location (not used) |
| title_tsvector | legacy search index (depreciated) |
| abstract | abstract text |

### **miniref** and **uniquename** example

If importing a paper from PubMED via a PMID, these two fields are automatically formatted. However, if you are manually entering or editing these fields, here is the convention:

| Field | Example |
|-------|-------------|
| title | The novel macrolide resistance genes mef(F) and msr(G) are located on a plasmid in Macrococcus canis and a transposon in Macrococcus caseolyticus |
| volumetitle | |
| volume | 76 |
| series_name | Journal of Antimicrobial Chemotherapy |
| issue | 1 |
| pyear | 2021 |
| pages | 48–54 |
| miniref | Fernandez, J.E. et al. 2021. Journal of Antimicrobial Chemotherapy 76(1):48–54. |
| uniquename | Fernandez, J.E. et al. 2021. The novel macrolide resistance genes mef(F) and msr(G) are located on a plasmid in Macrococcus canis and a transposon in Macrococcus caseolyticus. Journal of Antimicrobial Chemotherapy 76(1):48–54. |

### [bioRxiv](https://www.biorxiv.org) or DOI example

For bioRxiv pre-prints or if the citation is only available as a DOI, we use a slightly different format to support the DOI:

| Field | Description |
|-------|-------------|
| title | Molecular characterisation of, and antimicrobial resistance in, Clostridioides difficile from Thailand, 2017-2018 |
| volumetitle | |
| volume | DOI |
| series_name | bioRxiv |
| issue | |
| pyear | 2020 |
| pages | 10.1101/2020.11.12.379040 |
| miniref | Imwattana et al. 2020. bioRxiv DOI:10.1101/2020.11.12.379040 |
| uniquename | Imwattana et al. 2020. Molecular characterisation of, and antimicrobial resistance in, Clostridioides difficile from Thailand, 2017-2018. bioRxiv DOI:10.1101/2020.11.12.379040 |

## *pub_dbxref* table

The *pub_dbxref* table connects publications to external related data, specifically PMID accessions (*db_id*=46) or DOIs (*db_id*=73), via the [External Reference Module](dbxref_module.md).

| Field | Description |
|-------|-------------|
| pub_dbxref_id | unique identifier for each publication - dbxref association |
| pub_id | publication being attached to external accessions |
| dbxref_id | the connection to the [External Reference Module](dbxref_module.md) |
| is_current | flag for dbxref status, 0 = in use, 1 = obsolete (not used) |
