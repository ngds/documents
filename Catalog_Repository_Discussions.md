# NGDS Software brainstorming - Catalog and Repository Discussion
We want to take elements from all the best workflows, with the knowledge we've gained about how the system should work, as well as the state which data/metadata comes into a system in reality, for the best functioning system. One overview idea is: 
* 'staging repository' where data is initially brought in for some analysis, metadata QA/QC, or to provide feedback for a data provider
* modules can be built to support metadata/data QA/QC and standardization (GeoCoding, ExcelToNGDS tool, CSV to XML, etc)
* catalog system, where metadata can be managed and served out from a CSW endpoint
* an interface that ingests this data, harvests from other nodes, CSWs, and WAFs  
  
Below are some considerations and discussion for catalog systems and repository systems.

## Software landscape


## Catalogs

Some open source projects we should be aware of:
[GeoNode](http://geonode.org/), [GeoNetwork](http://geonetwork-opensource.org/), [OpenGeoPortal](http://opengeoportal.org/), and [ESRI Geoportal](https://github.com/Esri/geoportal-server/wiki) all implement various aspects of the functionality we need.  Here are some notes on each:

### [GeoNode](http://geonode.org/)
GeoNode is a web-based application and platform for developing geospatial information systems (GIS) and for deploying spatial data infrastructures (SDI). GeoNode is built on GeoServer.

* **Technology**: python, Django, Bootstrap, JQuery, PostGIS, GeoServer, pyCSW, openLayers, GeoExt. 
* **APIs**: 
  * Open Geospatial Consortium (OGC) Web Map Service (WMS), Web Feature Service (WFS), Web Coverage Service (WCS), and Catalogue Service for Web (CSW); 
  * GeoServer REST API, 
  * GeoNode search and REST APIs (I couldn't find any documentation)
* **Geonode Assessment**:  This software is essentially just a user-friendly wrapper around Geoserver.  It only supports one dimesional resources, meaning that we can't have a package with multiple resources in it.  Resources can only contain a single layer.  Geonode is a Django project and relies on the Django ORM for it's search capabilities.  Currently, there isn't any support for richer search engines like SOLR or Lucene.  The stack relies on PyCSW for serving the CSW and all data that is uploaded is indexed in the PyCSW database.  This is really the only selling point for us, as this is something that CKAN does not do.  But, Geonode provides no support for harvesting data from remote sources.  SO -- This project looks promising, but isn't mature enough to accomplish our goals.


### [GeoNetwork](http://geonetwork-opensource.org/)
Provides metadata editing and search functions,  an embedded interactive web map viewer. It is currently used in numerous Spatial Data Infrastructure initiatives across the world.  GeoNetwork is part of the [Open Source Geospatial Foundation (OSGeo)](http://www.osgeo.org) software stack

* web interface 
   *  search geospatial data across multiple local and distributed geospatial catalogues
   *  combine Web Map Services from distributed servers around the world in the embedded map viewer
   *  publish geospatial data using the online metadata editing tools and optionally the embedded GeoServer map server. 
* Administrators manage user and group accounts, configure the server through web based and desktop utilities and schedule metadata harvesting from other catalogs.
* Up- and downloading of files
* Online editing of metadata with a powerful template system
* Scheduled harvesting and synchronization of metadata between distributed catalogs
* Fine-grained access control with group and user management
* Multi-lingual user interface
* support for metadata standards: ISO19115/ISO19119/ISO19110 following ISO19139, FGDC, and Dublin Core
* **Technology**
	* Java, XSLT
* **APIs** 
	* OGC-CSW 2.0.2 ISO Profile client and server 
	* OAI-PMH client and server 
	* Z39.50 protocols
	* GeoRSS server
	* GEO OpenSearch server
	* WebDAV harvesting
	* GeoNetwork to GeoNetwork harvesting support
	* Map Services OGC-WMS, WFS, WCS, KML and others through the embedded GeoServer map server.
	* Custom API allows URL access to most GeoNetwork functions
* **GeoNetwork Assessment**: If we decide to rebuild this project from scratch, then GeoNetwork looks like it would be a great platform for building on top of.  Not only does it have a large user base and great documentation; the real selling point is that it's a Java web application which makes it platform independent out of the box.  Unfortunately, the fact that this is a Java application makes it a poor choice for NGDS because for years we have been building our tools with Python and JavaScript.  It wouldn't be impossible to make our tools compatible with GeoNetwork, but we'd either have to rely on Unix subprocesses or (I think) we'd have to host our Python/ JavaScript applications on separate ports and build a REST interface for them to communicate with GeoNetwork. - Geonetwork also has an organized metadata managment system that allows organizations to apply different profiles using XML templates and XSLT transformations. -Editing individual resources in this systems UI, like most other metadata catalogs, is not ideal. 


### [OpenGeoPortal](http://opengeoportal.org/)
web application to rapidly discover, preview, and retrieve geospatial data from multiple repositories. Open Geoportal is a front end to the geospatial data index in SOLR. Preview map services in web map viewer.
Amazon Machine Image (AMI) available for easy implementation. Technical overveiw: http://opengeoportal.org/wp-content/uploads/2013/10/Steve_McDonald_Chris_Barnett_OGPTechnicalIntroduction.pdf.  OpenGeoPortal doens't appear to have the metadata management capabilities like GeoNetwork or Geoportal.

* **Technology**
	* Tomcat, Java, JSP, Javascript, SOLR/Lucene, OpenLayers, JQuery, GeoServer, GeoWebCache, TileMill, relational db or file system

* **APIs**
	* SOLR API


* 
### [ESRI Geoportal](https://github.com/Esri/geoportal-server/wiki)
Geoportal Server allows you to catalog the locations and descriptions of your organization's geospatial resources in a central repository called a geoportal, which you can publish to the Internet or your intranet. Visitors to the geoportal can search and access these resources to use with their projects. If you grant them permission, visitors can also register geospatial resources with the geoportal. Geoportal provides xml templates, definitions, and transformation files that can be modified to alter the behavior of xml validation, allowing organizations to apply preferred profiles.
  * customizable metadata profiles
  * xml validation tools, xml file upload, and individual resource error handling
  * Harvest from an ESRI metadata service, Web-accessible folder, open Archive initiative service, CSW catalog, or THREDDS Data server catalog
  * Synchronization between all approved harvest sources

* **Technology**
	* Java, PostGIS, Lucene, Tomcat, Javascript, XML, XSLT, PostgreSQL

* **APIs**
	* CSW 
	* ESRI REST

* **ESRI GeoPortal comments**: AZGS currently uses this along with a custom OS application for metadata management and validation developed with Node.js and Apache CouchDB. CouchDB is the back-end for a Django repository UI. Use of CouchDB creates an opportunity to bulk edit metadata from a view of repository data before it is harvested into the catalog (ESRI GeoPortal) and is probably the biggest benefit from this system. Web services cannot be created, only metadata is manged with this system, so users would have to have a separate method of web service deployment. 

* **Metadata Management**: The Geoportal has been incredibly convenient for metadata management, having XML and XSLT documents available for customization has helped in implementing the USGIN-ISO profile.  This style of transformation has made taking outside distributed metadata and conforming it to our specs much easier.  Geoportal also preservers the xml document and gives somewhat detailed errors for each failed record. Geoportal and Ryans repo tool have consistently been the most used applications for metadata management. 


### [GI-cat](http://essi-lab.eu/do/view/GIcat/WebHome)
GI-cat is an implementation of a broker catalog service made by ESSI-Lab. It allows clients to discover and evaluate geoinformation resources over a federation of data sources. It publishes different catalog interfaces, allowing different clients to use the service. GI-cat features caching and mediation capabilities and can act as a broker towards disparate catalog and access services: by implementing metadata harmonization and protocol adaptation, it is able to transform query results to a uniform and consistent interface. GI-cat is based on a service-oriented framework of modular components and can be customized and tailored to support different deployment scenarios.

* **APIs**
	* [GI-cat catalog service](http://essi-lab.eu/do/viewfile/GIcat/GIcatDocumentation?rev=1;filename=GI-cat-6.0-specification-0.1.pdf)

### [CKAN](http://ckan.org/)
Data management system built on top of the Pylons web framework.  For data, it supports publishing, harvesting and searching.  It also supports metadata construction and editing.  For user experience, it supports a hierarchical user managament and organizations system.  CKAN's strengths primarily revolve around the unopinionated nature of the system.  Vanilla CKAN provides all of the basic needs of a data portal: database management, search interface, user management, data and metadata creation and data harvesting.  It has an API for builidng extensions and developers are expected to use and build extensions to engineer a data system tailored to fit the needs of specific organizations.  Writing CKAN extensions is not all that hard, either.  CKAN's weaknesses result from it's core design being one that aims to solve too many problems.  By attempting to solve all of the problems associated with managing data, CKAN is a very vanilla application and therefore requires a lot of development to solve the problems of individual organizations.  A specific example of this is how CKAN deals with databases.  CKAN relies on PostgreSQL, which is a relational database management system.  Relational databases are perfect for solving problems with user/organizational management where we want to build relationships between different data objects in a system.  But relational databases aren't so great for solving problems with actual documents of data.  Most datasets exist independently of one another and it makes a lot more sense to structure a database of documents with multi-dimesional hierarchies instead of a spider web of primary and foreign keys littered throughout the database tables.

* **Technology**:
	* Python, JavaScript, PostgreSQL, PostGIS, RabbitMQ, SOLR, Tomcat
* **APIS**:
	* CSW
	* REST
	* Celery
	* SOLR
	* OWSLib
	* SQLAlchemy
* **CKAN Assessment**:  CKAN is the safest system to choose because this is the software that our current developers have worked with most recently and for the longest amount of time.  Vanilla CKAN does not look like a bad system.  It has a lot of strengths and has a great interface for extending the system.  Plus, it's a Python/JavaScript project, so it's automatically compatible with the other USGIN/NGDS tools people have developed over the years. CON - metadata management is not okay!  PRO - Web service deployment using GeoServer and PostGIS; flexible enough to integrate model management.  If we can integrate some of the work (even conceptually) from the [custom metadata mangement tools Ryan built](https://github.com/ngds/documents/blob/master/Catalog_Repository_Discussions.md#state-geothermal-repository) for AZGS, this option would look far better. 
  
  
## Repositories

### [State Geothermal Repository](http://repository.stategeothermaldata.org)
AZGS currently uses Ryan's custom OS application for metadata management and validation developed with Node.js and Apache CouchDB. CouchDB is the back-end for a Django repository UI. Use of CouchDB creates an opportunity to bulk edit metadata from a view of repository data before it is harvested into the catalog (ESRI GeoPortal) and is probably the biggest benefit from this system. 

* **Technology**:
	* Node.js, JavaScript, CoucbDB (https://github.com/rclark/project-open-catalog)

* **comments**: Couch, Couch, Couch!  We know it. We like it. 

### [Neo4J Graph Database](http://projects.spring.io/spring-data-neo4j/)
Features
* Enterprise database (graph model for data representation)
* Uses relational database and other NOSQL alternatives
* Neo4j is flexible, yet, unlike other NOSQL databases, offers structure
* Uses Cypher (particular Graph query language)
* massively scalable, up to several billion nodes/relationships/properties
* accessible by REST interface or an object-oriented Java API
* can develop with Python or JavaScript (http://www.neo4j.org/develop)

Neo4J vs ElasticSearch discussion (http://kkovacs.eu/cassandra-vs-mongodb-vs-couchdb-vs-redis):
* Neo4J:
    Written in: Java  
    Main point: Graph database - connected data  
    License: GPL, some features AGPL/commercial  
    Protocol: HTTP/REST (or embedding in Java)  
    Standalone, or embeddable into Java applications  
    Full ACID conformity (including durable data)  
    Both nodes and relationships can have metadata  
    Integrated pattern-matching-based query language ("Cypher")  
    Also the "Gremlin" graph traversal language can be used  
    Indexing of nodes and relationships  
    Nice self-contained web admin  
    Advanced path-finding with multiple algorithms  
    Indexing of keys and relationships  
    Optimized for reads  
    Has transactions (in the Java API)  
    Scriptable in Groovy  
    Online backup, advanced monitoring and High Availability is AGPL/commercial licensed  
Best used: For graph-style, rich or complex, interconnected data. Neo4j is quite different from the others in this sense.  
* ElasticSearch
    Written in: Java  
    Main point: Advanced Search  
    License: Apache  
    Protocol: JSON over HTTP (Plugins: Thrift, memcached)  
    Stores JSON documents  
    Has versioning  
    Parent and children documents  
    Documents can time out  
    Very versatile and sophisticated querying, scriptable  
    Write consistency: one, quorum or all  
    Sorting by score (!)  
    Geo distance sorting  
    Fuzzy searches (approximate date, etc) (!)  
    Asynchronous replication  
    Atomic, scripted updates (good for counters, etc)  
    Can maintain automatic "stats groups" (good for debugging)  
    Still depends very much on only one developer (kimchy).   
Best used: When you have objects with (flexible) fields, and you need "advanced search" functionality.  

* **Neo4J comments**: Probably not geared at all toward our needs; our database has to be very schema-specific

### [MongoDB](https://www.mongodb.org/)
If scaleability is not a concern (not likely for us) CouchDB or MongoDB are good choices. Features:  
* NoSQL DB
* written in C++
* JSON-style documents with dynamic schemas
* full index support
* DB replication 
* document-based queries  

Thoughts on Couch vs Mongo:
* MongoDB: If you need dynamic queries. If you prefer to define indexes, not map/reduce functions. If you need good performance on a big DB. If you wanted CouchDB, but your data changes too much, filling up disks; Master-Slave Replication ONLY
* CouchDB : For accumulating, occasionally changing data, on which pre-defined queries are to be run. Places where versioning is important; Master-Master Replication
* CouchDB (http://kkovacs.eu/cassandra-vs-mongodb-vs-couchdb-vs-redis)
    Written in: Erlang  
    Main point: DB consistency, ease of use  
    License: Apache  
    Protocol: HTTP/REST  
    Bi-directional (!) replication,  
    continuous or ad-hoc,  
    with conflict detection,  
    thus, master-master replication. (!)  
    MVCC - write operations do not block reads  
    Previous versions of documents are available  
    Crash-only (reliable) design  
    Needs compacting from time to time  
    Views: embedded map/reduce  
    Formatting views: lists & shows  
    Server-side document validation possible  
    Authentication possible  
    Real-time updates via '_changes' (!)  
    Attachment handling  
    thus, CouchApps (standalone js apps)  
Best used: For accumulating, occasionally changing data, on which pre-defined queries are to be run. Places where versioning is important.  
For example: CRM, CMS systems. Master-master replication is an especially interesting feature, allowing easy multi-site deployments.   
* MongoDB (http://kkovacs.eu/cassandra-vs-mongodb-vs-couchdb-vs-redis)
    Written in: C++  
    Main point: Retains some friendly properties of SQL. (Query, index)  
    License: AGPL (Drivers: Apache)  
    Protocol: Custom, binary (BSON)  
    Master/slave replication (auto failover with replica sets)  
    Sharding built-in  
    Queries are javascript expressions  
    Run arbitrary javascript functions server-side  
    Better update-in-place than CouchDB  
    Uses memory mapped files for data storage  
    Performance over features  
    Journaling (with --journal) is best turned on  
    On 32bit systems, limited to ~2.5Gb  
    An empty database takes up 192Mb  
    GridFS to store big data + metadata (not actually an FS)  
    Has geospatial indexing  
    Data center aware   
Best used: If you need dynamic queries. If you prefer to define indexes, not map/reduce functions. If you need good performance on a big DB. If you wanted CouchDB, but your data changes too much, filling up disks.  
For example: For most things that you would do with MySQL or PostgreSQL, but having predefined columns really holds you back.   

* **Couch vs Mongo comments**: Seems that we don't really need the extra processing power that Mongo provides, and master-master DB replication that Couch provides can be really useful. We know Couch already, and like that the views can be constructed to do edits without anything predefined. Couch also offers views in a way that we can query and spot-check easily and quickly. Updating, Data Manipulations, and sorting are all pretty straight forward in Mongo and Couch.

###[Apache Cassandra](http://cassandra.apache.org/)
If scaleability is a concern, options like Cassandra exist (http://kkovacs.eu/cassandra-vs-mongodb-vs-couchdb-vs-redis).
*   Written in: Java
*   Main point: Store huge datasets in "almost" SQL
*   License: Apache
*   Protocol: CQL3 & Thrift
*   CQL3 is very similar SQL, but with some limitations that come from the scalability (most notably: no JOINs, no aggregate functions.)
*   CQL3 is now the official interface. Don't look at Thrift, unless you're working on a legacy app. This way, you can live   without understanding ColumnFamilies, SuperColumns, etc.
*   Querying by key, or key range (secondary indices are also available)
*   Tunable trade-offs for distribution and replication (N, R, W)
*   Data can have expiration (set on INSERT).
*   Writes can be much faster than reads (when reads are disk-bound)
*   Map/reduce possible with Apache Hadoop
*   All nodes are similar, as opposed to Hadoop/HBase
*   Very good and reliable cross-datacenter replication
*   Distributed counter datatype.
*   You can write triggers in Java.
Best used: When you need to store data so huge that it doesn't fit on server, but still want a friendly familiar interface to it.
For example: Web analytics, to count hits by hour, by browser, by IP, etc. Transaction logging. Data collection from huge sensor arrays.
* **Cassandra Comments**:  Cassandra in comparison to Couch lacks a lot of features we are used to. It does however have its own qualities, like being "almost" SQL reducing some of the learning curve compared to other options. The real downside I saw while testing out cassandra was that manipulating data and/or attempting to sort data was just not a smooth process.  


### Amazon: Files on EC2 with an S3 back-end
* Recommended by Ryan
* Super useful if concerned about scaleability
* Used instead of CouchDB or MongoDB, but need to build a separate spatial index; can index by keys, just a sequential list  

  
## Modules for 'staging repository' concept
