Contributed by Joe Bussell                                                                          
                                                                                                    
    The interface code in mapscript_wrap.c is broken for PERL 5.8.                                  
    Specifically, the macro XS(boot_mapscript) must be declared if the PERL object is not declared. 
                                                                                                    
    In my world line 431 which reads:                                                               
    SWIGEXPORT(void) boot_mapscript(CV* cv);                                                        
                                                                                                    
    should be replaced with:                                                                        
    XS(boot_mapscript);                                                                             
                                                                                                    
How does this bug manifest itself? In what mapserver version?                                       
                                                                                                    
My mapserver-4.2.3/mapscript/perl/mapscript_wrap.c:811 has                                          
{{{                                                                                                 
#!perl                                                                                              
 #define SWIG_init    boot_mapscript                                                                
                                                                                                    
 #define SWIG_name   "mapscriptc::boot_mapscript"                                                   
 #define SWIG_prefix "mapscriptc::"                                                                 
                                                                                                    
 #ifdef __cplusplus                                                                                 
 extern "C"                                                                                         
 #endif                                                                                             
 #ifndef PERL_OBJECT                                                                                
 #ifndef MULTIPLICITY                                                                               
 SWIGEXPORT(void) SWIG_init (CV* cv);                                                               
 #else                                                                                              
 SWIGEXPORT(void) SWIG_init (pTHXo_ CV* cv);                                                        
 #endif                                                                                             
}}}                                                                                                 
Which looks like it has some changes.                                                               
----                                                                                                
back to PerlMapScrip
