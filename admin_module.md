# Administrative Module

The following tables are used to support the Broad Street database.

[[_TOC_]]

## *users* table

This table stores all users or curators. There are two types of users: Curators (*type* = 1) can login and edit ontologies, while Regular (*type* = 0) users can login and use analysis tools without inputing captcha forms. When creating a new user the default is *type* = 0. New users can register at https://dune.mcmaster.ca/auth/register, but *type* can only be changed at the SQL-level by a CARD administrator.

| Field | Description |
|-------|-------------|
| id | unique user identifier |
| name | user name |
| email | user email |
| password | password (encrypted) |
| remember_token | cache token (encrypted) |
| created_at | timestamp for creation of user |
| updated_at | timestape for update of user |
| type | curator (1) or regular (0) |

## *password_resets* table

A table for tracking password resets by users.

| Field | Description |
|-------|-------------|
| id | unique user identifier from *users* table |
| email | user email |
| token | new password |
| created_at | timestamp for password reset |

## *activity_log* table

A table logging all curator transactions at http://dune.mcmaster.ca.

| Field | Description |
|-------|-------------|
| id | unique curator transaction identifier |
| user_id | curator from the *users* table |
| text | text describing the transaction |
| ip_address | IP address used by curator |
| created_at | timestamp of the transaction |
| updated_at | timestamp of any update to the transaction |

## *json_changesets* table

A table tracking all the change set files for *Track CARD Data Changes* at https://card.mcmaster.ca/download

| Field | Description |
|-------|-------------|
| json_changesets_id | unique identifier for each change set |
| old_version | version of CARD before changes |
| new_version | version of CARD after changes |
| created_at | timestamp for creation of change set |
| filepath | name of change set file, e.g. card-diff-v1.0.7-1.0.8.json |

## *json_downloads* table

A table listing all the download files for https://card.mcmaster.ca/download

| Field | Description |
|-------|-------------|
| json_downloads_id | unique identifier for each download file |
| version | data set version |
| created_at | timestamp for creation of download file |
| data | description of contents of download file |
| filepath | name of download file, e.g. prevalence-v3.0.5.tar.bz2 |
| type | type of download file, see below |
| file_formats | list of file formats in download |

### *json_downloads* types

| type | description |
|-------|-------------|
| 0 | canonical CARD data, e.g. broadstreet-v3.1.4.tar.bz2 |
| 1 | RGI software,  e.g. software-v5.2.1.tar.bz2 |
| 2 | preliminary version of prevalence data (depreciated) |
| 3 | RGI Conda package, e.g. RGI-linux.tar.gz |
| 4 | preliminary version of resistomes data, (depreciated) |
| 5 | ontologies, e.g. ontology-v3.1.4.tar.bz2 |
| 6 | resistomes, variants, prevalence data, e.g. prevalence-v3.0.5.tar.bz2 |
| 7 | bait capture platform, e.g. baits-v1.0.0.tar.gz |

## *download_log* table 

A table logging downloads at https://card.mcmaster.ca/download.

| Field | Description |
|-------|-------------|
| download_stats_id | unique download identifier  |
| email | email address provided (depreciated) |
| type | type of download (depreciated) |
| version | track version of download (depreciated) |
| created_at | timestamp for download |
| expires | (depreciated) |
| download_key | download key |
| ip | IP address used |
| link | file downloaded, e.g. download/1/software-v5.2.1.tar.bz2 |

## *api* table 

A table logging all API usage of http://card.mcmaster.ca.

| Field | Description |
|-------|-------------|
| id | unique website use identifier  |
| session | session cookie |
| intype | user input type, e.g. RGI |
| absolute_path | path to transaction files, results, uploads |
| url | URL for viewing results |
| response | result of API use, e.g. "success" |
| created_at | timestamp of API use |
| ip | IP address used |
| geo_area_code | geographic area code (if possible) |
| geo_area_name | geographic area name (if possible) |
| consent | [CARD:Live](https://card.mcmaster.ca/live) consent: 0 = no, 1 = yes |
| downloaded | results downloaded: 0 = no, 1 = yes |

## *card_admin* table

This is a depreciated table.