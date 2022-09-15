# Broad Street & Website Automated Tasks

Automated tasks are run using the CARD Website [crontab](https://devcard.mcmaster.ca:8888/root/card-website/-/blob/master/_crons/crontab).

[[_TOC_]]

## Update Website Search Box

This crontab job is used to genearate the *search.json* file which is used to populate the search bar on the CARD website. The underlying script is [updateTextSearch.pl](https://devcard.mcmaster.ca:8888/root/card-website/-/blob/master/_scripts/ontology_search/updateTextSearch.pl) and this search cron runs every 5 mins, so changes to ontology names, addition of terms, or changes to synonyms or CARD Short Names will work in the search bar within 5 minutes.

## Calculate *cvtermpath* table

This crontab job is used to create all the directed graph paths between ontology terms, runs once each day, and the underlying script is [calculatecvtermpath.pl](https://devcard.mcmaster.ca:8888/root/card-website/-/blob/master/_scripts/calculate_cvtermpath/calculatecvtermpath.pl). A number of functions on the CARD website depend on cvtermpath, so ontological changes will not take effect until ~6 am each day (cron job starts at 2 am). For details see the [*cvtermpath* table documentation](/database_modules/controlled_vocabulary_module.md). This script is accompanied by second script that outputs errors or warnings to SLACK at 7 am, [check_cvtermpath_calculations.sh](https://devcard.mcmaster.ca:8888/root/card-website/-/blob/master/_scripts/calculate_cvtermpath/check_cvtermpath_calculations.sh).

## Broadstreet QC - ARO, cvterms

This crontab job checks for errors in the [Antibiotic Resistance Ontology](/ontologies/antibiotic_resistance_ontology.md) and [Controlled Vocabulary Module](/database_modules/controlled_vocabulary_module.md), runs at 2 am every day, and the underlying script is [broadstreet_qc.pl](https://devcard.mcmaster.ca:8888/root/card-website/-/blob/master/_scripts/quality_control/broadstreet_qc.pl).

## Broadstreet QC - Models

This crontab job checks for errors in the [Model Module](/database_modules/model_module.md), runs at 2 am every day, and the underlying script is [models_qc.php](https://devcard.mcmaster.ca:8888/root/card-website/-/blob/master/_scripts/quality_control/models_qc.php).

## Generate *card.json* and BLAST databases

This crontab job runs at 1 am every day, creating fresh versions of the *card.json* and BLAST databases for the CARD website. The underlying script is [create_analysis_dbs.php](https://devcard.mcmaster.ca:8888/root/card-website/-/blob/master/_scripts/generate_analysis_databases/create_analysis_dbs.php).

## Update Publications

This crontab job is used to update publication entries with Epub ahead of print and No authors listed in the [Publication Module](publication_module.md). This cron runs every first (1st) of the month, and the underlying script is [updatepubmodule.pl](https://devcard.mcmaster.ca:8888/root/card-website/-/blob/master/_scripts/pub_module/updatepubmodule.pl).

## CARD*Shark2 and CARD*Shark3

These crontab jobs run the [CARD*Shark2](https://devcard.mcmaster.ca:8888/sharmaan/CARDshark2) and [CARD*Shark3](https://devcard.mcmaster.ca:8888/arman/cardshark3) algorithms on the first of every month to list new papers for curator review.

## CARD:Live

On the public website only, these cronjobs run gather (every 5 minutes) and process (every 6 minutes) the [CARD:Live](https://card.mcmaster.ca/live) data from online use of RGI.

## Site Maintenance Mode

These crontab job starts and stops the website maintenance mode at 2 am and 4 am, respectively, using the script [site.php](https://devcard.mcmaster.ca:8888/root/card-website/-/blob/master/_scripts/maintenance/site.php). This places the website in "CARD is Down" status so curators cannot edit with the *cvtermpath* table is calculated.

## Cleanup Webserver Files

This crontab job removes files that are 7 days old from _session, _temp, _cache (including RGI and BLAST results), runs each day at 12:01 am, and the underlying script is [clean.php](https://devcard.mcmaster.ca:8888/root/card-website/-/blob/master/_scripts/cleanup/clean.php). 

## Artisan cleanup compiled views

This crontab job removes compiled view files that are 7 days old, runs each day at 12:02 am, and the underlying script is [artisan.php](https://devcard.mcmaster.ca:8888/root/card-website/-/blob/master/_scripts/cleanup/artisan.php). 

## Create Prevalence Data Files

This crontab job creates the *genomes.json* file that enable the Resistomes, Variants, and Prevalence webpages, runs every first (1st) of the month, and the underlying script is [create_genomes_data.php](https://devcard.mcmaster.ca:8888/root/card-website/-/blob/master/_scripts/generate_prevalence_data/create_genomes_data.php). 

## Create Violin Plots

This crontab job creates the Resistomes & Variants violin plots, e.g. see [TEM-1's violin plots](https://devcard.mcmaster.ca/prevalence/355), at 2 am each day and the underlying script is [create_models_violins.php](https://devcard.mcmaster.ca:8888/root/card-website/-/blob/master/_scripts/generate_prevalence_data/create_models_violins.php). 

