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
* **CKAN Assessment**:  CKAN is the safest system to choose because this is the software that our current developers have worked with most recently and for the longest amount of time.  Vanilla CKAN does not look like a bad system.  It has a lot of strengths and has a great interface for extending the system.  Plus, it's a Python/JavaScript project, so it's automatically compatible with the other USGIN/NGDS tools people have developed over the years. CON - metadata management is not okay!  PRO - Web service deployment using GeoServer and PostGIS; flexible enough to integrate model management.  If we can integrate some of the work (even conceptually) from the custom metadata mangement tools Ryan built for AZGS, this option would look far better. 
  
  
## Repositories

### [State Geothermal Repository](http://repository.stategeothermaldata.org)
AZGS currently uses Ryan's custom OS application for metadata management and validation developed with Node.js and Apache CouchDB. CouchDB is the back-end for a Django repository UI. Use of CouchDB creates an opportunity to bulk edit metadata from a view of repository data before it is harvested into the catalog (ESRI GeoPortal) and is probably the biggest benefit from this system. 

* **Technology**:
	* Node.js, JavaScript, CoucbDB (https://github.com/rclark/project-open-catalog)  

### [Neo4J Graph Database](http://projects.spring.io/spring-data-neo4j/)
Features
* Enterprise database (graph model for data representation)
* Uses relational database and other NOSQL alternatives
* Neo4j is flexible, yet, unlike other NOSQL databases, offers structure
* Uses Cypher (particular Graph query language)
* massively scalable, up to several billion nodes/relationships/properties
* accessible by REST interface or an object-oriented Java API
* can develop with Python or JavaScript (http://www.neo4j.org/develop)

### MongoDB

### Amazon: Files on EC2 with an S3 back-end
* Recommended by Ryan
* Super useful if concerned about scaleability
* Used instead of CouchDB or MongoDB, but need to build a separate spatial index; can index by keys, just a sequential list  

  
## Modules for 'staging repository' concept
