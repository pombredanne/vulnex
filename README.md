vulnex
======

Building and running
--------------------

* Requires Docker (https://www.docker.com/)
* Run `docker-compose up`
* The to use the tool go to http://localhost:3001/

Components
----------

* client: The web-client of the VA tool, served by the server component
* crawler: `crawler.sh` downloads all Maven based GitHub repositories of the Eclipse foundation
* db-connector: The database connector module for the db-importer and server
* db-importer: `db-importer.jar` creates the database used for the VA tool
* server: The server of the VA tool, serving the static content and REST API


Crawling data
-------------

### Requirements

* JDK 8u231
* Apache Maven 3.6.3
* Docker (`docker` and `docker-compose`)

### Preparation

1. Setup **Eclipse Steady**
  * Clone `https://github.com/eclipse/steady.git`
  * Build with `mvn clean install -DskipTests` (see https://github.com/eclipse/steady)
  * Create and deploy the Docker container (see https://eclipse.github.io/steady/admin/tutorials/docker/)


2. Fill the vulnerability database
  * Clone `https://github.com/SAP/vulnerability-assessment-kb.git`
  * Run the `import_vulas_kb.sh` script (see https://github.com/SAP/vulnerability-assessment-kb)

### Execute

Change the `vulas.core.space.token` to the token of your **Eclipse Steady** workspace in the files `vulas.part.txt` and `vulas.profile.txt`.

To run the full crawling and database creation process run:

```
./crawler.sh -a
```

This will create and populate all required directories and databases for the VA tool.

#### Parameters of `crawler.sh`

You can run parts of the pipeline by specifying one of the following flags.

```
usage: $./crawler.sh [-adhprs]
-a      Run all steps of the crawling process
-c      Create the database file
-d      Download all repositories in the repos.txt file
-e      Gather extra data from GitHub and LGTM
-h      Display this help
-p      Modify and process pom.xml files of all repositories
-r      Reset and update all repositories, removing all modifications
-s      Synchronize the repository list
-t      Reset all pom.xml files
```

---
Author: Frederik Dennig

Copyright 2019
