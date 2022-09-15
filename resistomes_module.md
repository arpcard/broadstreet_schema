# Resistomes & Variants Module

The Resistomes & Variants modules contains computer-generated resistome predictions for important pathogens, including sequence variants beyond those reported in the scientific literature, as well as prevalence statistics for AMR genes among pathogens, genomes, and plasmids. Antimicrobial resistance (AMR) molecular resistome, variant, and prevalence data are generated using the Resistance Gene Identifier ([RGI](https://github.com/arpcard/rgi)), a tool for putative AMR gene detection from submitted sequence data using the AMR detection models available in CARD. The contents of the Resistomes & Variants Module tables are calculated outside of CARD and uploaded to Broad Street *en masse* for CARD releases. Full details about these data can also be found on the [CARD website](https://card.mcmaster.ca/resistomes).

## *prevalence* table

A table containing all individual RGI predictions (excluding "Loose") for genomes, plasmids, genomic islands, or whole-genome shotgun assemblies from NCBI or private data sets. 

| Field | Description |
|-------|-------------|
| prevalence_id | unique identifier for each RGI prediction |
| model_id | CARD detection model from the [Model Module](model_module.md), eg. model_id=783 for the *NDM-1* gene |
| source_species | the taxonomic source of the annotated sequence from the NCBI Taxonomy Ontology |
| dna_accession | the annotated sequence accession, e.g. NZ_CP084031.1 |
| fmin | the start nucleotide position of the RGI prediction in the annotated sequence in [Interbase Co-Ordinates](model_module.md#interbase-co-ordinates) |
| fmax | the stop nucleotide position of the RGI prediction in the annotated sequence in [Interbase Co-Ordinates](model_module.md#interbase-co-ordinates) |
| strand | forward (1) or reverse (-1) strand of RGI prediction |
| evalue | [expectation value](https://www.ncbi.nlm.nih.gov/BLAST/tutorial/Altschul-1.html) of the RGI prediction (depreciated) |
| snps | observed AMR SNPs (depreciated) |
| criteria | RGI paradigm (Strict or Perfect) |
| data_type | annotated sequence data type, see below |
| bitscore | [bitscore](https://www.ncbi.nlm.nih.gov/BLAST/tutorial/Altschul-1.html) of the RGI prediction |
| identity | [percent identity](https://www.ncbi.nlm.nih.gov/BLAST/tutorial/Altschul-1.html) of the RGI prediction |
| hsp_length | length of the [High Scoring Pair](https://www.ncbi.nlm.nih.gov/BLAST/tutorial/Altschul-1.html) of the RGI prediction (depreciated) |
| created_at | timestamp of RGI analysis |
| assembly | NCBI assembly accession |
| biosample | NCBI biosample accession |

### *prevalence* data types

| data_type | Description |
|-------|-------------|
| ncbi_chromosome | NCBI complete genome sequences |
| ncbi_contig | NCBI whole-genome assembly contigs |
| ncbi_gi | IslandViewer genomic islands found in NCBI |
| ncbi_plasmid | NCBI complete plasmid sequences |

## *prevalence_sequence* table

A table containing all **unique** AMR allele sequences predicted by RGI.

| Field | Description |
|-------|-------------|
| prevalence_sequence_id | unique identifier for each unique AMR allele sequence |
| dna_sequence | DNA sequence 5' -> 3', including start and stop codons |
| protein_sequence | Protein sequence |
| created_at | timestamp of RGI analysis |

## *prevalence_prevalence_sequence* joins table connects the 

The *prevalence_prevalence_sequence* table connects the individual RGI predictions in the *prevalence* table with the unique AMR allele sequences in the *prevalence_sequence* table. A joins table is used as RGI predictions from different sources (e.g. an *E. coli* genome and an *A. baumannii* plasmid) may encode the identical AMR allele sequence.

| Field | Description |
|-------|-------------|
| prevalence_prevalence_sequence_id | unique identifier for each prevalence prevalence_sequence join |
| prevalence_id | associated prevalence identifier found in the *prevalence* table |
| prevalence_sequence_id | associated prevalence_sequence identifier found in the *prevalence_sequence* table |
| created_at | timestamp of RGI analysis |

## *prevalence_models_stats* table (a [materialization](https://en.wikipedia.org/wiki/Materialized_view))

A table containing all prevalence statistics for individual CARD detection model from the [Model Module](model_module.md).

| Field | Description |
|-------|-------------|
| prevalence_models_stats_id | unique identifier for each statistic |
| model_id | CARD detection model from the [Model Module](model_module.md) |
| source_species | the taxonomic source of the annotated sequence from the NCBI Taxonomy Ontology |
| ncbi_plasmid | prevalence in NCBI complete plasmid sequences |
| ncbi_contig | prevalence in NCBI whole-genome assembly contigs |
| ncbi_chromosome | prevalence in NCBI complete genome sequences |
| created_at | timestamp for materialization |
| criteria | prevalence is from Perfect only (perfect) or Perfect & Strict hits combined (perfect_strict) |
| ncbi_gi | prevalence in IslandViewer genomic islands found in NCBI |

## *prevalence_categories_stats* table (a [materialization](https://en.wikipedia.org/wiki/Materialized_view))

A table containing all prevalence statistics for individual CARD Classification Tags, i.e. drug classes, resistance mechanisms, AMR gene families, etc.

| Field | Description |
|-------|-------------|
| prevalence_categories_stats_id | unique identifier for each statistic |
| source_species | the taxonomic source of the annotated sequence from the NCBI Taxonomy Ontology |
| category_id | ontology term *cvterm_id* from the Antibiotic Resistance Ontology, e.g. *cvterm_id*=35935 for aminoglycoside antibiotic |
| criteria | prevalence is from Perfect only (perfect) or Perfect & Strict hits combined (perfect_strict) |
| value | prevalence value |
| created_at | timestamp for materialization |
| cvterm_class | Classification Tag, e.g. Drug Class |

## *prevalence_denominator* table (a [materialization](https://en.wikipedia.org/wiki/Materialized_view))

A table containing all sample sizes for each *data_type*.

| Field | Description |
|-------|-------------|
| prevalence_denominator_id | unique identifier for each statistic |
| source_species | the taxonomic source of the annotated sequence from the NCBI Taxonomy Ontology |
| data_type | annotated sequence data type, see above |
| value | sample size |
| created_at | timestamp for materialization |

## *prevalence_cvterm* joins table (a [materialization](https://en.wikipedia.org/wiki/Materialized_view))

The *prevalence_cvterm* table connects the individual RGI predictions in the *prevalence* table with Antibiotic Resistance Ontology categories used in table *prevalence_categories_stats*.

| Field | Description |
|-------|-------------|
| prevalence_cvterm_id | unique identifier for each prevalence cvterm_id join |
| prevalence_id | associated prevalence identifier found in the prevalence table |
| cvterm_id | category term *cvterm_id* from the Antibiotic Resistance Ontology, e.g. *cvterm_id*=35935 for aminoglycoside antibiotic |
| created_at | timestamp for materialization |

## *prevalence_accession_crossref* table

This is a depreciated table.

## *collection_metadata* table

This is a depreciated table.

## *bmx_nucleotide_sequences* table

This is a depreciated table.
