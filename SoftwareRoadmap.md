# NGDS Software brainstorming

NGDS requires software to support a variety of functions. This document is for brainstorming requirements/use scenarios. The [Enterprise view](#enterprise-view) focuses on domain operations (attempt to be independent of software). The [Functional View](#functional-view) section decomposes the operations into units that can be implemented in software components.

NGDS is implementing the US GeoScience Information network framework for scientific data sharing. It is a sub-network focused on content related to geothermal systems, and geothermal energy exploration and development.


## Enterprise view

### Registry
1. create and manage metadata documenting the information resources under the stewardship of the organization
2. metadata in the registry should be available for harvesting into catalogs through various interfaces; at least WAF and CSW
3. We are using USGIN ISO metadata profile.

### Repository
1. Provide a repository for long term preservation of digital information and work products associated with projects.
2. Organize and manage repository items into logical collections to facilitate management and browsing of content.
3. Selectively make repository items discoverable and accessible to outside users, with group level access controls to metadata describing the items and to the items (metadata may be discoverable for items that requires special permissions to access).
4. Enable bulk parsing of standardized metadata from Excel to ISO XML

### Catalog
1. Assemble that metadata in a searchable catalog to allow members of the organization to find and access information
2. Harvest metadata for selected resources into the catalog
3. Provide user friendly search interface with Map-oriented search, and a text-and-facet oriented search.

### Data Access
1. Users want to be able to download data in convenient formats (csv, .shp, gml, kml, GeoJSON, "Add to GoogleEarth", "Add to ArcMap", "Add to QGIS" or some other OSGS)
2. Download should work across multiple services if they are interoperable
3. Enable viewing XML metadata record for each resource
4. Should be a live link to contact data source provider

### Installable node
1. For beginning system participation, barrier to entry should be low, so we want to provide an installable software package to implement basic metadata creation and publication for harvest, file repository, and data service deployment.
2. Create standardized web services along with metadata
3. Manual external file validation for data for services; automatic validation of data for services files
3. Upload standardized bulk metadata from Excel spreasheet (which will be parsed into ISO XML)
4. Ensuring meaningful URIs for each record of an uploaded standardized dataset, adhereing to NGDS URI standards (having name authorities, tokens) already in place, creating URL redirections for those URIs

### Federated information system operation
1. need to be able to notify aggregators of publishers that would like to be harvested
2. need registry of information exchange specifications, vocabularies, system nodes...
3. URI dereferencing for identifiers
4. notification and issue tracking, user feedback

The tools should be easy to use, standards-based, and free-open source. Software enabling these capabilities should be available in a package with a simple install procedure.

## Functional View
* Create USGIN metadata
* Edit USGIN metadata
  * Table and form views of metadata
* Bulk update metadata (e.g. replace all occurrences of e-mail address A with address B)
* Upload files to managed file system for long term preservation (checksums)
  * publish to AZGS only or to public
* Automatic conversion of bulk uploaded files (CSV) to ISO XML
* Export packages consisting of bundled repository item (RI) and metadata
* versioning of repository items; 
    * URL for current version, and distinct URLs for specific version
* repository items grouped in collections (one RI may be in many collections, collections are hierarchical)
  * access control, publication options at item and collection level.
* harvest csv, ISO from WAF into repository
  * endpoints to harvest particular collections
* harvest ISO, FGDC, DC from CSW into repository
    * validation, accept flawed records for update, fixing
* expose CSW, openSearch, OAIPMH for harvest, search by others
  * endpoints to access particular collections
  * USGIN ISO, DC, DCAT output formats to start
* members, editors, owner, public roles
* groups of members, hierarchical groups
* access control to collections at member and group level


# Software landscape
Some open source projects we should be aware of:

NGDS has chosen to work with the CKAN platform for enabling these capabilities. Version 1 of the NGDS Node-in-a-box (NIAB) implements a basic suit of the necessary capabilities.  Other CKAN branches like data.gov include functionality that we want to include in our build. 

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
* **GeoNetwork Assessment**: If we decide to rebuild this project from scratch, then GeoNetwork looks like it would be a great platform for building on top of.  Not only does it have a large user base and great documentation; the real selling point is that it's a Java web application which makes it platform independent out of the box.  Unfortunately, the fact that this is a Java application makes it a poor choice for NGDS because for years we have been building our tools with Python and JavaScript.  It wouldn't be impossible to make our tools compatible with GeoNetwork, but we'd either have to rely on Unix subprocesses or (I think) we'd have to host our Python/ JavaScript applications on separate ports and build a REST interface for them to communicate with GeoNetwork.

### [OpenGeoPortal](http://opengeoportal.org/)
web application to rapidly discover, preview, and retrieve geospatial data from multiple repositories. Open Geoportal is a front end to the geospatial data index in SOLR. Preview map services in web map viewer.
Amazon Machine Image (AMI) available for easy implementation

* **Technology**
	* Tomcat, Java, JSP, Javascript, SOLR/Lucene, OpenLayers, JQuery, GeoServer, GeoWebCache, TileMill, relational db or file system

* **APIs**
	* SOLR API

### [ESRI Geoportal](https://github.com/Esri/geoportal-server/wiki)
Geoportal Server allows you to catalog the locations and descriptions of your organization's geospatial resources in a central repository called a geoportal, which you can publish to the Internet or your intranet. Visitors to the geoportal can search and access these resources to use with their projects. If you grant them permission, visitors can also register geospatial resources with the geoportal. Geoportal provides xml templates, definitions, and transformation files that can be modified to alter the behavior of xml validation, allowing organizations to apply preferred profiles.
  * customizable metadata profiles
  * xml validation tools, xml file upload, and individual resource error handling
  * Harvest from an ESRI metadata service, Web-accessible folder, open Archive initiative service, CSW catalog, or THREDDS Data server catalog
  * Synchronization between all approved harvest sources

* **Technology**
	* Java, PostGIS, Lucene

* **APIs**
	* CSW, 
	* ESRI REST


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
* **CKAN Assessment**:  CKAN is the safest system to choose because this is the software that our current developers have worked with most recently and for the longest amount of time.  Vanilla CKAN does not look like a bad system.  It has a lot of strengths and has a great interface for extending the system.  Plus, it's a Python/JavaScript project, so it's automatically compatible with the other USGIN/NGDS tools people have developed over the years.
