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
3. Validate XML while or before harvesting
4. Provide user friendly search interface with Map-oriented search, and a text-and-facet oriented search.

### Data Access
1. Users want to be able to download data in convenient formats (csv, .shp, gml, kml, GeoJSON, "Add to GoogleEarth", "Add to ArcMap", "Add to QGIS" or some other OSGS)
2. Download should work across multiple services if they are interoperable
3. Enable viewing XML metadata record for each resource
4. Should be a live link to contact data source provider

### Installable node
1. For beginning system participation, barrier to entry should be low, so we want to provide an installable software package to implement basic metadata creation and publication for harvest, file repository, and data service deployment.
2. Create standardized web services along with metadata; schema resolution of WFS requests desireable 
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
* Create schema-compliant web services
* validate web services for schema-compliance
* Integrate current information exchange (model) management
* members, editors, owner, public roles
* groups of members, hierarchical groups
* access control to collections at member and group level


