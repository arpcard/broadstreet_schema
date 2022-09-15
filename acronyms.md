# List of CARD Terms & Acronyms

| Acronym | Definition |
|-----|-----|
| *API* | Application Programming Interface, see [Wikipedia](https://en.wikipedia.org/wiki/API) |
| *ARO* | The [Antibiotic Resistance Ontology](/ontologies/antibiotic_resistance_ontology.md) |
| *baits* or *AMR baits* | A metagenomic AMR bait capture platform based on data in CARD, see [Capturing the Resistome: a targeted capture method to reveal antibiotic resistance determinants in metagenomes](https://pubmed.ncbi.nlm.nih.gov/31611361/), the [code](https://devcard.mcmaster.ca:8888/balcock/amr-baits), and the [official downloads](https://card.mcmaster.ca/download) |
| *BLAST* | The Basic Local Alignment Search Tool underlying the [Resistome Gene Identifier](https://github.com/arpcard/rgi), see [Wikipedia](https://en.wikipedia.org/wiki/BLAST_(biotechnology)) |
| *Broad Street* | The postgreSQL data schema underlying CARD, see [Modules](README.md) |
| *CARD* | The [public version](http://card.mcmaster.ca) of the Comprehensive Antibiotic Resistance Database |
| *CARD Live* | The CARD:Live project collects pathogen identification, MLST, AMR gene list, date, and geographical region for genome sequences submitted to the [Resistome Gene Identifier](https://github.com/arpcard/rgi) online, providing a dynamic view of antibiotic resistant isolates being analyzed around the world. See the [live data](https://card.mcmaster.ca/live), [code](https://card.mcmaster.ca/live), and [dashboard code](https://github.com/arpcard/card-live-dashboard) |
| *CARD MIC* | A collection of antibiogram and MIC breakpoint data for antimicrobial resistance genes, see the [MIC Module](mic_module.md) |
| *CARD-R*, *CARD:Resistomes* or *CARD Resistomes & Variants* | Computer-generated resistome predictions for important pathogens, including sequence variants beyond those reported in the scientific literature, as well as prevalence statistics for AMR genes among pathogens, genomes, and plasmids. See the [data](https://dune.mcmaster.ca/resistomes), [code](https://devcard.mcmaster.ca:8888/balcock/resistomes), and [Resistomes Module](resistomes_module.md) |
| *CARD:Prev* or *CARD:Prevalence* | A materialization of CARD:Resistomes data detailing prevalence of AMR genes or drug classes impacted, see the [data](https://dune.mcmaster.ca/prevalence), [code](https://devcard.mcmaster.ca:8888/balcock/resistomes), and [Resistomes Module](resistomes_module.md)  |
| *CARD Shark*  | Text-mining algorithms for literature-guided curation, see [CARD*Shark2](https://devcard.mcmaster.ca:8888/sharmaan/CARDshark2) and [CARD*Shark3](https://devcard.mcmaster.ca:8888/arman/cardshark3) |
| *CARD-QC* or *QC* | Quality control algorithms for the contents of CARD, see the [CARD-QC repo](https://devcard.mcmaster.ca:8888/amcarthur/CARD-QC) |
| *Chado* | The original data schema used by CARD, see the [CARD history](/README.md) |
| *cvterm* or *cvterm_id* | An acronym for an ontology (or controlled vocabulary) term encoded in the [cvterm table](https://devcard.mcmaster.ca:8888/root/broadstreet/-/blob/mastercontrolled_vocabulary_module.md#cvterm-table) |
| *cvtermpath* | The ontological path between two ontology terms, encoded in the [cvtermpath table](https://devcard.mcmaster.ca:8888/root/broadstreet/-/blob/mastercontrolled_vocabulary_module.md#cvtermpath-table-a-materialization) |
| *devCARD* | The [private curator & developer version](http://dune.mcmaster.ca) of the Comprehensive Antibiotic Resistance Database |
| *dune* | The server hosting *devCARD* |
| *EPI* | The Efflux Pump Identifier software, see [Identifying novel β-lactamase substrate activity through in silico prediction of antimicrobial resistance](https://pubmed.ncbi.nlm.nih.gov/33416461/) and the associated [GitHub repo](https://github.com/karatsang/rulesbased_logisticregression) |
| *ETD* | The [Emergent Threat Detector](https://github.com/fmaguire/evolving_threat_detector), a pilot project for identifying changes in resistome relative to closest relatives and contextualising results |
| *FACOMPARE* | An algorithm in the [CARD-QC repo](https://devcard.mcmaster.ca:8888/amcarthur/CARD-QC/-/tree/master/facompare) to compare the contents of [CARD](http://card.mcmaster), [Resfinder](https://cge.cbs.dtu.dk/services/ResFinder/), [NCBI AMR](https://www.ncbi.nlm.nih.gov/bioproject/PRJNA313047), and other databases | 
| *GenEpiO* | The [Genomic Epidemiology Ontology](https://genepio.org), which uses a number of [Antibiotic Resistance Ontology](/ontologies/antibiotic_resistance_ontology.md) terms |
| *GraphViz* or *DOT files* | [GraphViz](https://www.graphviz.org) is a software package for visualizing relationships by reading DOT-format files. These can be generated for any ontology at [DevCARD Tools](https://dune.mcmaster.ca/tools/generatejson). Here is an [example image](/img/CTX-M-15.jpg) |
| *hAMRonization* | A [workflow](https://github.com/pha4ge/hAMRonization_workflow) for standardizing output from many AMR gene prediction tools |
| *IRIDA* | The [Integrated Rapid Infectious Disease Analysis](https://irida.ca) platform, which uses the [Resistance Gene Identifier](https://github.com/arpcard/rgi) |
| *JSON* | A file format for storing and sharing complex data, see [Wikipedia](https://en.wikipedia.org/wiki/JSON) |
| *MO* | The [Model Ontology](/ontologies/model_ontology.md) |
| *MOBIO* | The [Mobilome Ontology](/ontologies/mobile_elements_ontology.md), also know as the Mobile Elements Ontology |
| *mobility risk factor* | The [CARD Mobility Risk Factor repository](https://devcard.mcmaster.ca:8888/amcarthur/card-mobility-risk-factor) explores metrics possible based on the CARD:Resistomes data |
| *NCBI* or *NCBITaxon* | The [National Center for Biotechnology Information](https://www.ncbi.nlm.nih.gov) at the National Library of Medicine (USA), the source of all sequence data for CARD. NCBITaxon specifically refers to the NCBI's organism taxonomy resources, see the [NCBI Taxonomy Ontology](/ontologies/ncbi_taxonomy_ontology.md) |
| *Orthogonality* | The re-using of terms in one ontology within another ontology, see [Orthogonality](/ontologies/orthogonality.md) |
| *PDB* | [Protein Data Bank](https://www.rcsb.org), a source of protein structures |
| *PMID* or *PubMED* | [PubMED](https://pubmed.ncbi.nlm.nih.gov) is a repository of publications at the National Library of Medicine (USA) and the PubMED Identifier (PMID) is a unique identifier for each publication |
| *PubCHEM* | [PubCHEM](https://pubchem.ncbi.nlm.nih.gov) is a repository of chemical structures at the National Library of Medicine (USA) and the PubChem Identifier is a unique identifier for each structure |
| *RGI* | CARD's Resistance Gene Identifier software, see the [GitHub repo](https://github.com/arpcard/rgi) |
| *RGI bwt* | Resistance Gene Identifier module for read-based metagenomics AMR annotation, see the [GitHub repo](https://github.com/arpcard/rgi) |
| *RGI kmers* or *CARD kmers* | Resistance Gene Identifier module for taxonomic & genomic identification kmers, see the [GitHub repo](https://github.com/arpcard/rgi) |
| *RO* | The [Relationship Ontology](/ontologies/relationship_ontology.md) |
| *SNP* | Single-Nucleotide Polymorphism, see [Wikipedia](https://en.wikipedia.org/wiki/Single-nucleotide_polymorphism) |
| *Violin Plot* | A [violin plot](https://en.wikipedia.org/wiki/Violin_plot) is a method of plotting numeric data, see an example for [MCR-1.1 Resistomes & Variants](https://dune.mcmaster.ca/prevalence/2203) |
| *VIRO* | The [Virulence Ontology](/ontologies/virulence_ontology.md) |
| *Wild CARD* | Nickname for the *CARD:Resistomes* [code](https://devcard.mcmaster.ca:8888/balcock/resistomes) and [Resistomes Module](resistomes_module.md) |
