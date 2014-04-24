## Harvesting the NGDS CKAN CSW in ESRI Geoportal 

NGDS implements the pycsw OGC CSW server plugin for CKAN, configuration to the Geoportal will need to be done for versions older than 1.2.5 to support a pycsw server. 

The ESRI Geoportal provides individual profiles for different CSW implementations, the profiles are necessary for harvesting from CSW endpoints because each CSW implementation has different protocols for making GetRecords requests and receiving responses. To obtain the pycsw profile for Geoportal, update to version 1.2.5 or add and configure the new files in Geoportal. 

The configuration files required to implement the pycsw profile are available in the ‘develop’ branch on [ESRI’s Geoportal-Server GitHub repository.](https://github.com/Esri/geoportal-server/tree/develop/geoportal/src/gpt/search/profiles)

Place these three files, available from link above, into the profiles directory:
 …\geoportal\WEB-INF\classes\gpt\search\profiles

*  ``` CSW_2.0.2_APISO_pyCSW_GetRecordByID_Response.xslt```
*  ``` CSW_2.0.2_APISO_pyCSW_GetRecords_Request.xslt```
*  ``` CSW_2.0.2_APISO_pyCSW_GetRecords_Response.xslt```

Add the pyCSW profile definition into the CSWProfiles.xml file:
 …\geoportal\WEB-INF\classes\gpt\search\profiles\CSWProfiles.xml

```
<Profile>
		<ID>urn:ogc:CSW:2.0.2:HTTP:APISO:pycsw</ID>
		<Name>pycsw</Name>
		<CswNamespace>http://www.opengis.net/cat/csw/2.0.2</CswNamespace>
		<Description/>
		<GetRecords>
			<XSLTransformations>
				<Request>CSW_2.0.2_APISO_pyCSW_GetRecords_Request.xslt</Request>
				<Response>CSW_2.0.2_APISO_pyCSW_GetRecords_Response.xslt</Response>
			</XSLTransformations>
		</GetRecords>
		<GetRecordByID>
			<RequestKVPs><![CDATA[service=CSW&request=GetRecordById&version=2.0.2&ElementSetName=full&outputschema=http%3A%2F%2Fwww.isotc211.org%2F2005%2Fgmd]]></RequestKVPs>
			<XSLTransformations>
				<Response>CSW_2.0.2_APISO_pyCSW_GetRecordByID_Response.xslt</Response>
			</XSLTransformations>
		</GetRecordByID>
		<SupportSpatialQuery>True</SupportSpatialQuery>
		<SupportContentTypeQuery>False</SupportContentTypeQuery>
		<SupportSpatialBoundary>True</SupportSpatialBoundary>
	</Profile>
```

Add new search link file types into the gpt.properties file:
 …\geoportal\WEB-INF\classes\gpt\resources\gpt.properties 
```
# Search link file types:
catalog.rest.link.addToMap     = Add to Map
catalog.rest.link.csv          = CSV
catalog.rest.link.doc          = DOC
catalog.rest.link.gml          = GML
catalog.rest.link.json         = JSON
catalog.rest.link.geojson      = GeoJSON
catalog.rest.link.getmap       = GetMap
catalog.rest.link.getcoverage  = GetCoverage
catalog.rest.link.html         = WWW
catalog.rest.link.kml          = KML
catalog.rest.link.pdf          = PDF
catalog.rest.link.ppt          = PPT
catalog.rest.link.shp          = SHP
catalog.rest.link.sos          = SOS
catalog.rest.link.txt          = TXT
catalog.rest.link.wms          = WMS
catalog.rest.link.wfs          = WFS
catalog.rest.link.wcs          = WCS
catalog.rest.link.xls          = XLS
catalog.rest.link.xml          = XML
catalog.rest.link.zip          = ZIP
```


## Changes for Harvesting the NGDS Ckan CSW into Geoportal
To support harvest from the NGDS pycsw endpoint, the original logic for Geoportals pycsw GetRecords Request ```CSW_2.0.2_APISO_pyCSW_GetRecords_Request.xslt ``` was modified slightly. A small snippet was added to the first ```</xsl:otherwise>``` statement in the GetRecords Request xslt for cases when original Filter Query might fail.
```
<xsl:otherwise>
	<ogc:PropertyIsLike escapeChar="!" singleChar="#" wildCard="*">
		<ogc:PropertyName>apiso:Title</ogc:PropertyName>
		<ogc:Literal>*</ogc:Literal>
	</ogc:PropertyIsLike>
</xsl:otherwise>
```






