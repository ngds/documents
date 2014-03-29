#AZGS Outline of NGDS Requirements#

Compiled and summarized by Ryan Clark 2012-10-22 for design discussions with Siemens.

##System Architecture Requirements##
* The system consists of the following components
  * a set of standards, content models and protocols that allow the various components in the system to function interoperably
  * a network of distributed nodes that provide geothermal data following the standards and protocols defined by the system
  * a variety of applications built to search for, or consume data using the system’s standards and protocols
  * a searchable catalog that maintains a registry of data provided by all the nodes in the network
  * a mechanism for creating and dereferencing unique identifiers (or URIs) for resources in the system. “Resources” include metadata records, datasets, individual rows within a dataset, and physical objects
  *a web-application that serves as an single entry point to the system in order to establish and maintain system identity and branding
*  Data in the system is categorized in three distinct groups, or tiers
  * Tier 1: unstructured data such as scanned well logs, geologic maps, physical resources (e.g. core, rock or water samples, etc.), or peer-reviewed scientific articles
  * Tier 2: tabular, structured, georeferenced data that does not conform to a content model
  * Tier 3: tabular, structured, georeferenced data that does conform to a content model
*  The system defines a set of content models for common geothermal data themes and provides a canonical encoding of these models
*  The system defines a standard model for metadata describing data in the system and provides a canonical encoding of that model
*  The system defines a set of protocols used to exchange data, metadata and other information between components of the system
*  File-based data is transferred via standard HTTP protocols (e.g. GET), or through person-to-person communication in the case of physical or otherwise offline resources
*  Tier 2 data can be transferred via standard Open Geospatial Consortium (OGC) web services (e.g. Web Feature Service or WFS, Web Map Service or WMS, Web Coverage Service or WCS)
*  Tier 3 data are expected to be transferred via standard Open Geospatial Consortium (OGC) web services (e.g. Web Feature Service or WFS, Web Map Service or WMS, Web Coverage Service or WCS)
*  Metadata is transferred via a standard OGC web service, Catalog Service for the Web or CSW
System Components Targeted for Software Development
*  An NGDS Node application, or node-in-a-box, which assists a data provider in sharing their data using the appropriate NGDS standards and protocols, as well as in describing their data using the system’s metadata standards
*  A web-application capable of creating and dereferencing URIs for resources in the system
*  A single, aggregating catalog that maintains a registry of data-providing nodes in the system, and provides a single point of search for data from the entire system
*  A user-centered, entry-point web-application where data consumers go to find, evaluate, explore and acquire data in the NGDS
##Overall Software Design Requirements##
*  SRS069: Software to be developed shall
  *  be based on industry-standard platforms and communications frameworks
  *  utilize a widely-used FOSS database system
  *  expose user-interfaces based on common interface conventions and user-centered design feedback
* SRS078: Shall be designed for hardware independence
* All software developed as part of this project shall be open-source (this isn’t in the requirements doc, at least not as a numbered requirement) and licensed under the xxx license.
Overall Software Maintainability and Supportability Requirements
*  SRS022: Source code shall have comments at least on a per-class level
*  SRS025: Software’s cyclic complexity check shall show values below a certain [what value???] threshold
*  SRS026: Software’s architecture shall be documented
*  SRS027: Software configuration parameters shall be documented
*  SRS028: Source code shall be covered by unit tests
*  SRS063: Written using the standard coding style for the used programming languages
*  SRS064: Written using the standard naming conventions for the used programming languages
*  SRS065: Shall be designed utilizing the concept of encapsulation. Components shall be created that encapsulate related functionality within them, and nothing more.
*  SRS066: Shall be modular to minimize the time and complexity involved in maintaining and extending the platforms and applications
*  SRS067: Shall not contain any statically detectable dead code
*  SRS068: Software development and modification work shall be maintained according to SCR’s standard quality assurance and configuration management processes
*  _There need to be more here about maintainability after the project moves out of SCR’s hands... i.e. supporting open-source community involvement_
##Overall Software Usability Requirements##
*  SRS032: Graphical user interfaces shall use a uniform look-and-feel defined by the UX team
*  SRS033: Tool-tips for interface controls needing user input (isn’t that part of the uniform look and feel to be determined?)
*  SRS034: Provide online help explaining how to perform user-related functions
*  SRS035: Clear, understandable and accurate information explaining each function that can be performed using the software
*  SRS036: Clear and understandable error messages explaining errors during user interaction
*  SRS041: Indicate if it has taken action in response to all user operations within 2 seconds
*  SRS049: None of the software’s functionality shall consume so much CPU or memory that is degrades any other functionality of the software
##Overall Software Security Requirements##
*  SRS050: Shall embody a security plan and process to ensure that unauthorized users are denied access
*  SRS057: Data communications between external systems and NGDS applications shall be secured by message authentication where applicable/necessary (I don’t get it)
*  SRS062: Developed considering good security coding practices, thus minimizing vulnerability to attacks
NGDS Node Application Requirements
##Requirements imposed by NGDS Architecture##
Note that this set of requirements must be filled by any system that wishes to play the role of a node providing data to the NGDS and is not specific to the NGDS Node application to be developed

*  Provide access to tier 1 data via standard HTTP protocols
*  Provide access to tier 2 and 3 data through WFS, WMS and WCS, where appropriate (satisfies SRS070, SRS071, SRS073, SRS074)
*  Construct metadata according to the standard NGDS metadata model (satisfies SRS002 and part of SRS077)
*  SRS077: Provide access to metadata through the CSW protocol
*  SRS007: Create URIs for all applicable data resources - open for discussion is whether that URI dereferencing application should be a global system component with nodes interacting via a service interface (as sort of implied by this document), or if the URI engine should be a part of the NGDS Node application
##Software Usability Requirements##
*  SRS029 (re-worded): Simple to set up as cloud-ready VM or through local installation routine with well-documented and minimal configuration in either case.
*  SRS030: Includes detailed instructions that guide the user through the installation process and joining the new node to the network
*  SRS031: Automatic updates?
*  SRS037: Key import operations should be transactional so as to allow users to abort operations before completion without any negative consequences
*  SRS038: Provide status indicator showing progress towards completion or user triggered processing, search queries, exports and downloads
Administrative Requirements
*  SRS019: Administrator can manage users
*  UC_030 and UC_031: add/remove
*  US_032: assign roles, which include
  * Data submitter
  * Data steward
  * Data originator
  * System administrator
*  SRS019a: Administrator can manage groups of users with shared access privileges
*  UC_034: Administrator can request registration in the aggregating catalog described below
*  UC_035: Administrator can upgrade NGDS Node application to an updated version of the software
*  UC_033: Users can email administrator
Security Requirements
*  SRS019c, SRS052 and SRS053: System-level access control to uploaded files to prevent unauthorized tampering or access
*  SRS020 and SRS021: System-level permissions assigned to each user role
*  SRS054: Only data stewards are allowed to delete resources
*  SRS056: Authenticated client-server communication will occur through encrypted HTTPS protocol
##Resource Accessibility Requirements##

A resource is a bundle including metadata describing the contained data, as well as any uploaded data files and processed data in a data store
*  Resources created by a data submitter are marked as submitted
*  SRS058: metadata and data access to submitted resources is restricted to those with appropriate permissions
*  submitted metadata is not included in CSW interface
*  UC_009: Mark resource public
*  public metadata included in CSW interface
*  SRS058: provides access to datasets uploaded as part of a public resource to anonymous users
##Metadata Input Requirements##
*  SRS022: All metadata creation and updating requires appropriate permission to edit the record
*  UC_003: Create metadata record through a form
  *  user information used to populate metadata contact information
*  SRS009: Suggest users select from a list of pre-defined geothermal thematic, and location-based keywords
*  UC_004: Bulk upload metadata from metadata template table
  *  allow upload of .csv files according to a standard format
  *  new records marked as submitted
  *  workflow for checking submitted records and marking as published described above
*  UC_005: Bulk update of metadata records
  *  select records based on a query or by collection
  *  workflow to select content item to update and provide value to replace and new value to use
*  SRS019b: Grouping of metadata records into collections with access control at the user or group level
Validation and Quality Assurance Requirements
*  (Part of UC_003 and UC_004): Automated validation of all new and edited records for:
  *  completeness
  *  URL checking
*  SRS006 and SRS011: de-duplication
*  Generate Validation / Quality Assurance report for each record
  *  UC_042: View Validation / Quality Assurance reports
  *  UC_043: Manually flag quality issues
  *  UC_047: Browse flagged entries
  *  UC_044: Perform manual error correction
  *  UC_045: Clear quality issue flag
*  SRS004: Send email alerts to data submitters notifying them of unpublished data
*  SRS005: Data submitters can turn off notifications on publication alerts
*  SRS013: Email data stewards and data submitters notifying them of QA flags
*  SRS014: Data stewards can turn off notifications on QA flag alerts
Data Submission Requirements
*  SRS022: All data uploading and updating requires appropriate permission to edit the record
*  SRS045: Shall be able to handle the import of data files up to 2GB in size
*  SRS046: Shall be able to handle up to 1000 data files in any one import operation
*  SRS047 and SRS048: Shall be able to handle storing up to 100,000 data files or up to 500 GB (whichever is met first) in the import directory
*  SRS008: Process supported file types to extract information to assist creation of metadata records
*  UC_006 and SRS003: Process data file in NGDS content model template
  *  validate data schema
*  SRS010: Perform error checking on content of data
  *  load data into data store
  *  deploy data as standard NGDS service (i.e. WFS, WMS)
*  SRS011: De-duplication of data on a row-by-row basis
*  Create logs during any data or metadata creation or update or QA procedure. Details include:
  *  time of activity
  *  actions taken
  *  user responsible
  *  data size
  *  user comments
*  from SRS010: indicate issues with data in known NGDS content models:
  *  quality issues
  *  missing values
  *  data type mismatch
  *  cardinality issues
*  UC_042: Validation / Quality Assurance reports as described above
*  UC_007 (and UC_046): View data logs
###requires authorization###
*  UC_008: Browse and manage resource directory
  *  user can view all resources under their stewardship or that they have submitted
  *  presented in a tree-view structure
  *  user can define collections
  *  access control can be assigned at collection level
  *  display indicates resources with quality issue flags
###Implicit Requirements Supporting NGDS Node Functionality###
*  UC_001: Login (also SRS051)
*  UC_002: Logout (also SRS051)
*  User self-registration, with authorization required by system administrator
*  SRS042: Support up to 50 simultaneous, authenticated, logged-in users
*  SRS043: Capable of handling a least 50 HTTP requests per minute
*  SRS044: Shall respond to every request in no more than 10 seconds (unrealistic for large data requests via web-services)—Notify user if a data access operationis likely to take longer?
*  SRS001: Provide a database for local storage of managed metadata and for datasets that are published via web services
*  SRS072: Shall utilize an abstraction layer for access to databases used for metadata management and management of data in NGDS content models  [this needs to be clarified..]
*  SRS023 and SRS061: To-be-defined logging of NGDS node functions. Maintain said log.
*  SRS024: Ongoing, scheduled checks on OGC services to indicate “Up” or “Down” status and contributing to quality assurance reports
*  SRS059: Maintain integrity and availability of data stored in local data store
*  SRS060: Maintain integrity of all files stored in the file repository
##Aggregating Catalog Requirements##

Requirements imposed by NGDS Architecture

*  Maintain registry of nodes in the NGDS
*  SRS040: Shall be able to maintain a list of at least 100 other system nodes
*  UC_036: Register new nodes
*  UC_036: Delete nodes from the NGDS
*  SRS015, SRS075: Harvest all published metadata from any registered node via CSW
*  SRS016: Aggregate metadata catalog accessible via standard, searchable protocols
  *  CSW
  *  Open Search
  *  OAI-OMH
  *  RSS / Atom Feeds
##Administrative Requirements##
*  UC_037: Manage user accounts for making registry adjustments
*  UC_039: Email node administrators
##Other Requirements##
*  validate metadata for 
  *  conformance with system standards
  *  check URLs included in metadata records for validity, flag bad records
  *  de-duplication
  *  mechanism to provide feedback from validation to metadata providers
  *  support for maintaining resource ratings
  *  support for maintaining user supplied tagging
##Entry-Point Application Requirements##

Requirements imposed by NGDS Architecture

*  SRS075: Perform search against the Aggregating Catalog or any other NGDS catalog described above using standard NGDS protocols (i.e. CSW)
*  SRS017: Search Functionality Requirements
*  UC_014: Map-based search. This would include the following sub-items, which would allow the user to quickly pan the map to the appropriate location based on a:
  *  UC_013: Landmark-based search
  *  UC_015: Coordinate-based search
  *  UC_016: Keyword-based search
*  UC_017: Browse / view search results. Display:
  *  on the map by location
  *  as human-readable text and links (UC_028) describing clearly how the data itself can be accessed
*  Filter search results...
  *  UC_018: by selecting a sub-area on the map
  *  UC_020: by attributes of the data themselves 
  *  Through facets, including:
      *  UC_019: by specific data type, perhaps content models?
      *  by a set of location terms
      *  by a set of geothermal thematic terms
      *  by source organization
      *  by publication date
##Metadata Record Evaluation Requirements##
*  UC_026: View entire metadata record as:
*  SRS076: System standard metadata encoding
  *  Atom Feed entry
  *  Human-readable HTML
*  UC_027: Export metadata in same formats listed above
*  UC_021: Get own measurement (I definitely don’t understand this requirement, but I think this is where it would fit)
*  UC_022: Triangulate with other sources (don’t get this either, perhaps implying adding other layers to the online map such as power-lines, land-use restrictions?)
*  Rate the quality / utility of an individual record through:
  *  comments
  *  0-5 star system
*  UC_024: Consider peer ratings (should either be implement peer rating system, make clear how is different from UC_022; is this a user reputation system?); use case is not consider something, its do something. Project management will consider whether to prioritize that use case.
##Search Result Sharing Requirements##
*  UC_010: Save current search
*  UC_011: Subscribe to new data that satisfies a saved search. Notify user via:
  *  email
  *  RSS / Atom Feed
*  UC_012: Load previous search criteria
*  Share search results with others via:
  *  UC_023: email
  *  Facebook
  *  Twitter
  *  Google+
  *  Reddit
  *  Yammer
*  SRS018 and UC_025: Data Visualization Requirements
*  SRS074: Access data from nodes using standard NGDS protocols
*  Options to download the entire dataset
  *  in native format for tier 1 data
  *  data
*  SRS018a: Filter the tier 2 and 3 data and download a subset in formats outlined above
*  Visualize as an HTML table
*  Visualize on a map
##URI Engine Requirements##

Requirements Imposed by NGDS Architecture

*  To-be-determined: Inherent component of NGDS Node application, or standalone system component?
*  Standalone system component implies requirement for a service-based approach to managing URIs that can be utilized by instances of the NGDS Node application
*  Generates URIs based on a standard NGDS syntax
Authentication Requirements
*  URIs categorized by namespaces, which are likely to correlate closely with instances of NGDS Node applications
*  User authentication mechanism with permissions to create/edit tied to namespaces
Functional Requirements
*  URIs support regular expression pattern based redirection
*  URIs implement HTTP content-negotiation for retrieval of different representations of the same resource
