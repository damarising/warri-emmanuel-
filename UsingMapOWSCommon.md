mapowscommon implements the OGC OWS Common Specification                                                                                                                                                                                                                                         
                                                                                                                                                                                                                                                                                                 
- version 1.0.0                                                                                                                                                                                                                                                                                  
- OGC document: [http://portal.opengeospatial.org/files/?artifact_id=8798 OGC 05-008]                                                                                                                                                                                                            
                                                                                                                                                                                                                                                                                                 
The library uses libxml2 (http://www.xmlsoft.org/) to create                                                                                                                                                                                                                                     
XML objects.                                                                                                                                                                                                                                                                                     
                                                                                                                                                                                                                                                                                                 
The library uses the tree and parser modules of libxml2.                                                                                                                                                                                                                                         
                                                                                                                                                                                                                                                                                                 
The library returns [http://www.xmlsoft.org/html/libxml-tree.html#xmlNodePtr xmlNodePtr] objects to the calling code.                                                                                                                                                                            
                                                                                                                                                                                                                                                                                                 
With the exception of msOWSCommonExceptionReport, ALL functions return an XML chunk.  It is the responsibility of the calling code to setup the root element with correct namespace decls and schema pointers                                                                                    
                                                                                                                                                                                                                                                                                                 
msOWSCommonExceptionReport returns an entire, complete and valid XML document, as it is the only XML in OWS Common which is designed to be implemented standalone.  In this case, the calling code must instantiate an [http://www.xmlsoft.org/html/libxml-tree.html#xmlNewDoc xmlNewDoc] object.
                                                                                                                                                                                                                                                                                                 
== EXAMPLE 1: create a service Exception ==                                                                                                                                                                                                                                                      
                                                                                                                                                                                                                                                                                                 

```                                                                                                                                                                                                                                                                                              
#include "libxml/parser.h"                                                                                                                                                                                                                                                                       
#include "libxml/tree.h"                                                                                                                                                                                                                                                                         
                                                                                                                                                                                                                                                                                                 
#include "mapowscommon.h"                                                                                                                                                                                                                                                                        
                                                                                                                                                                                                                                                                                                 
static int msSOSException(mapObj *map, char *locator, char *exceptionCode) {                                                                                                                                                                                                                     
  xmlDocPtr  psDoc      = NULL;                                                                                                                                                                                                                                                                  
  xmlNodePtr psRootNode = NULL;                                                                                                                                                                                                                                                                  
  xmlChar    *buffer    = NULL;                                                                                                                                                                                                                                                                  
  int size = 0;                                                                                                                                                                                                                                                                                  
                                                                                                                                                                                                                                                                                                 
  /* create XML document object */                                                                                                                                                                                                                                                               
  psDoc = xmlNewDoc(BAD_CAST "1.0");                                                                                                                                                                                                                                                             
                                                                                                                                                                                                                                                                                                 
  /* generate a service exception object */                                                                                                                                                                                                                                                      
  psRootNode = msOWSCommonExceptionReport(msEncodeHTMLEntities(msOWSGetSchemasLocation(map)), pszSOSVersion, msOWSGetLanguage(map, "exception"), exceptionCode, locator, msEncodeHTMLEntities(msGetErrorString("\n")));                                                                          
                                                                                                                                                                                                                                                                                                 
  /* set the root element of the document */                                                                                                                                                                                                                                                     
  xmlDocSetRootElement(psDoc, psRootNode);                                                                                                                                                                                                                                                       
                                                                                                                                                                                                                                                                                                 
  /* set the namespace of the document */                                                                                                                                                                                                                                                        
  xmlSetNs(psRootNode,  xmlNewNs(psRootNode, BAD_CAST MS_OWSCOMMON_OWS_NAMESPACE_URI, BAD_CAST MS_OWSCOMMON_OWS_NAMESPACE_PREFIX));                                                                                                                                                              
                                                                                                                                                                                                                                                                                                 
  msIO_printf("Content-type: text/xml%c%c",10,10);                                                                                                                                                                                                                                               
                                                                                                                                                                                                                                                                                                 
  /* dump the libxml2 object to a string for output */                                                                                                                                                                                                                                           
  xmlDocDumpFormatMemoryEnc(psDoc, &buffer, &size, "ISO-8859-1", 1);                                                                                                                                                                                                                             
                                                                                                                                                                                                                                                                                                 
  msIO_printf("%s", buffer);                                                                                                                                                                                                                                                                     
                                                                                                                                                                                                                                                                                                 
  /*free buffer and the document */                                                                                                                                                                                                                                                              
  xmlFree(buffer);                                                                                                                                                                                                                                                                               
  xmlFreeDoc(psDoc);                                                                                                                                                                                                                                                                             
                                                                                                                                                                                                                                                                                                 
  /* clear error since we have already reported it */                                                                                                                                                                                                                                            
  msResetErrorList();                                                                                                                                                                                                                                                                            
                                                                                                                                                                                                                                                                                                 
  return MS_FAILURE;                                                                                                                                                                                                                                                                             
}                                                                                                                                                                                                                                                                                                
```                                                                                                                                                                                                                                                                                              
                                                                                                                                                                                                                                                                                                 
See mapogcsos.c:msSOSException for an example in the codebase                                                                                                                                                                                                                                    
                                                                                                                                                                                                                                                                                                 
== Example 2: integrate OWS Common info as part of an existing document: ==                                                                                                                                                                                                                      
                                                                                                                                                                                                                                                                                                 

```                                                                                                                                                                                                                                                                                              
#include "libxml/parser.h"                                                                                                                                                                                                                                                                       
#include "libxml/tree.h"                                                                                                                                                                                                                                                                         
                                                                                                                                                                                                                                                                                                 
#include "mapowscommon.h"                                                                                                                                                                                                                                                                        
                                                                                                                                                                                                                                                                                                 
static int msPrintBoundingBox() {                                                                                                                                                                                                                                                                
  xmlDocPtr  psDoc      = NULL;                                                                                                                                                                                                                                                                  
  xmlNsPtr   psNs       = NULL;                                                                                                                                                                                                                                                                  
  xmlNodePtr psRootNode = NULL;                                                                                                                                                                                                                                                                  
  xmlNodePtr psNode     = NULL;                                                                                                                                                                                                                                                                  
  xmlChar    *buffer    = NULL;                                                                                                                                                                                                                                                                  
  int size = 0;                                                                                                                                                                                                                                                                                  
                                                                                                                                                                                                                                                                                                 
  /* create XML document object */                                                                                                                                                                                                                                                               
  psDoc = xmlNewDoc(BAD_CAST "1.0");                                                                                                                                                                                                                                                             
                                                                                                                                                                                                                                                                                                 
  /* create the namespace object */                                                                                                                                                                                                                                                              
  psNs  = xmlNewNs(psRootNode, BAD_CAST MS_OWSCOMMON_OWS_NAMESPACE_URI, BAD_CAST MS_OWSCOMMON_OWS_NAMESPACE_PREFIX);                                                                                                                                                                             
                                                                                                                                                                                                                                                                                                 
  /* generate the root node */                                                                                                                                                                                                                                                                   
  psRootNode = xmlNewNode(NULL, BAD_CAST "My_Root_Node");                                                                                                                                                                                                                                        
                                                                                                                                                                                                                                                                                                 
  /* set the root element of the document */                                                                                                                                                                                                                                                     
  xmlDocSetRootElement(psDoc, psRootNode);                                                                                                                                                                                                                                                       
                                                                                                                                                                                                                                                                                                 
  /* set the namespace of the document */                                                                                                                                                                                                                                                        
  xmlSetNs(psRootNode, psNs);                                                                                                                                                                                                                                                                    
                                                                                                                                                                                                                                                                                                 
  psNode = msOWSCommonWGS84BoundingBox(psNs, 2, -180.0, -90.0, 180, 90);                                                                                                                                                                                                                         
                                                                                                                                                                                                                                                                                                 
  msIO_printf("Content-type: text/xml%c%c",10,10);                                                                                                                                                                                                                                               
                                                                                                                                                                                                                                                                                                 
  /* dump the libxml2 object to a string for output */                                                                                                                                                                                                                                           
  xmlDocDumpFormatMemoryEnc(psDoc, &buffer, &size, "ISO-8859-1", 1);                                                                                                                                                                                                                             
                                                                                                                                                                                                                                                                                                 
  msIO_printf("%s", buffer);                                                                                                                                                                                                                                                                     
                                                                                                                                                                                                                                                                                                 
  /*free buffer and the document */                                                                                                                                                                                                                                                              
  xmlFree(buffer);                                                                                                                                                                                                                                                                               
  xmlFreeDoc(psDoc);                                                                                                                                                                                                                                                                             
}                                                                                                                                                                                                                                                                                                
```                                                                                                                                                                                                                                                                                              
                                                                                                                                                                                                                                                                                                 
See mapogcsos.c:msSOSGetCapabilities for an example in the codebase                                                                                                                                                                                                                              
