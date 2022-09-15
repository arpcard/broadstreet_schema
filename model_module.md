# Model Module

The Broad Street Model Module records the type of protein or nucleotide model used to detect an AMR gene and stores all related sequence and mutation reference information. Full descriptions of each model type can be found under the [Model Ontology](/ontologies/model_ontology.md) documentation. Each AMR detection model must be attached to an ARO ontology term; unattached (orphaned) models are not allowed.

## Interbase Co-Ordinates

*Sections of text below are copied from http://gmod.org/wiki/Introduction_to_Chado#Interbase_Coordinates*

Broad Street tracks nucleotide or amino acid sequence spacing using interbase coordinates. Interbase counts spaces, not bases. It is a 0-based coordinate system; counting starts at zero. This coordinate system is appealing for a variety of mathematical reasons. Standard (e.g. Genbank, BLAST) coordinate systems do not allow for a natural representation of zero-length features. An example of a zero-length feature is a cleavage site that is between, but does not include, 2 amino acids. Using interbase coordinates the total number of base pairs in a feature is calculated by subtracting the starting coordinate from the ending coordinate, without the need for an off-by-1 addition required by other coordinate systems. In general, much mathematical utility is enabled by the number zero.

![Interbase](/img/interbase.jpg)

*Interbase coordinates (top) and base-oriented (below): the position of the ATG in interbase is [3, 6] (between the 3rd and 6th gaps), while the position of the ATG in base coordinates is [4, 6] (between 4th and 6th bases inclusive).*

In the Model Module, fmin and fmax are interbase coordinates, whereas GenBank uses base coordinates. The coordinates in GenBank are fmin+1 and fmax. The length of the sequence in the Model Module = fmax - fmin, whereas in base coordinates it would be stop - start + 1.

## *models* table

The *models* table defines each model and its model type, as described by the [Model Ontology](/ontologies/model_ontology.md). 

| Field | Description |
|-------|-------------|
| model_id | unique identifier for each model |
| model_type_id | defines the assigned model type; for example, a protein homolog model is [model_type_id = 40292](https://dune.mcmaster.ca/ontology/40292) |
| name | the model name must be identical the assigned ontology term name |
| description | is a user-defined description; the default description is the ontology term description |
| private | indicates if the model is private (1) or not (0); private models are not visible on the CARD website |
| resistance_type | 0 = undefined, 1 = intrinsic (i.e., chromosomally-encoded), 2 = acquired (e.g., plasmid-bourne) |

Note: The [devCARD Tools Interface](https://dune.mcmaster.ca/tools/generatejson) has an option to include private models in a generated RGI-compatible JSON file.

## *model_parameters* table

Each model is associated with bioinformatics parameters for use by the Resistance Gene Identifier or other software, as described by the [Model Ontology](/ontologies/model_ontology.md). For example, most models (including [protein homolog](https://dune.mcmaster.ca/ontology/40292) and [protein variant]((https://dune.mcmaster.ca/ontology/40293)) models) have a BLASTP-determined cutoff E-value; protein variant models additionally have mutation (SNP, indel, etc.) parameters. Any given model may have multiple parameters associated with it if required.

| Field | Description |
|-------|-------------|
| model_parameters_id | unique identifier for each model parameter |
| type_id | defines the assigned parameter type; for example, a SNP is [model_type_id = 36301](https://dune.mcmaster.ca/ontology/36301) |
| model_id | associated model identifier found in the *models* table |
| value | the value of the parameter, regardless of parameter type |
| experimental | defines if the parameter is clinically validated (0) or *in vitro* (i.e. correlative) only (1) |

## *model_parameters_pub* joins table

This table documents the publications describing the individual model parameters, e.g. SNPs for a model must have a publication where they were originally described. Note that BLASTP E-values or other cut-off values do not appear in this table as they are determined algorithmically.

| Field | Description |
|-------|-------------|
| model_parameters_pub_id | unique identifier for each model parameter publication join |
| pub_id | publication identifier found in the [Publication Module](publication_module.md) |
| model_parameters_id | parameter identifier found in the *model_parameters* table |
| model_id | associated model identifier found in the *models* table |

## *model_sequences* table

Most, but not all, models are based on reference sequences acquired from NCBI. This table records these sources for CARD reference sequences using Interbase Co-Ordinates (see above).

| Field | Description |
|-------|-------------|
| model_sequences_id | unique identifier for each model sequence |
| model_id | associated model identifier found in the *models* table |
| dna_sequence | reference DNA sequence |
| protein_sequence | reference protein sequence (if applicable) |
| source_species | taxonomic source of the reference sequence(s) as defined by the [NCBI Taxonomy Ontology](/ontologies/ncbi_taxonomy_ontology.md) |
| fmin | start position of reference sequence in original NCBI entry |
| fmax | end position of reference sequence in original NCBI entry |
| dna_dbxref_id | connection to the [External Reference Module](dbxref_module.md) for the NCBI accession of the original nucleotide NCBI entry, including version |
| protein_dbxref_id | connection to the [External Reference Module](dbxref_module.md) for the NCBI accession of the original protein NCBI entry (if applicable), including version |
| strand | indicates if the strand in original NCBI entry is 5'->3' (1) or 3'->5' (-1) |
| partial | 0 = coding DNA sequence (CDS) is complete, 1 = CDS is partial |

Regardless of the original NCBI sequence data, protein coding reference *dna_sequence* are stored in Broad Street from start codon (5') to stop codon (3'), inclusive. As such, the stored *dna_sequence* should always translate 5' to 3' to the stored *protein_sequence*. In addition, partial CDS should only include the codons such that the stored *dna_sequence* should translate to the stored *protein_sequence*, whether or not the partial CDS includes a stop codon. Collectively, *dna_dbxref_id*, *fmin*, *fmax*, and *strand* can be used to map the stored *dna_sequence* back to the original NCBI entry; they are not used directly by CARD.

## *cvterm_models* joins table

Models are associated with ontology terms in the [Controlled Vocabulary Module](database_modules/controlled_vocabulary_module.md) via the following joins table:

| Field | Description |
|-------|-------------|
| cvterm_models_id | unique identifier for each cvterm models join |
| cvterm_id | associated cvterm identifier found in the [Controlled Vocabulary Module](database_modules/controlled_vocabulary_module.md) |
| model_id | associated model identifier found in the *models* table |
