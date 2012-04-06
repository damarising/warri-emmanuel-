The annotations layer provides an excellent mechanism for automatically placing labels and symbols.  For some uses, however, you may want a symbol with effectively no label at all.  Unfortunately, many mechanisms for attempting this result in neither the symbol nor the label to display.  The following method allows for no (visible) label while still rendering symbols at automatically selected locations.
                                                                                                                                                                                                                                                                                                                                                                                                                      

```                                                                                                                                                                                                                                                                                                                                                                                                                   
LAYER                                                                                                                                                                                                                                                                                                                                                                                                                 
  NAME "shields"                                                                                                                                                                                                                                                                                                                                                                                                      
  DATA shield_file                                                                                                                                                                                                                                                                                                                                                                                                    
  STATUS default                                                                                                                                                                                                                                                                                                                                                                                                      
  TYPE annotation                                                                                                                                                                                                                                                                                                                                                                                                     
  CLASS                                                                                                                                                                                                                                                                                                                                                                                                               
    TEXT ' '                                                                                                                                                                                                                                                                                                                                                                                                          
    STYLE                                                                                                                                                                                                                                                                                                                                                                                                             
      SYMBOL [shieldtype]                                                                                                                                                                                                                                                                                                                                                                                             
    END                                                                                                                                                                                                                                                                                                                                                                                                               
    LABEL                                                                                                                                                                                                                                                                                                                                                                                                             
      MINFEATURESIZE auto                                                                                                                                                                                                                                                                                                                                                                                             
      MINDISTANCE 10                                                                                                                                                                                                                                                                                                                                                                                                  
      REPEATDISTANCE 250                                                                                                                                                                                                                                                                                                                                                                                              
      POSITION CC                                                                                                                                                                                                                                                                                                                                                                                                     
    END                                                                                                                                                                                                                                                                                                                                                                                                               
  END                                                                                                                                                                                                                                                                                                                                                                                                                 
END                                                                                                                                                                                                                                                                                                                                                                                                                   
```                                                                                                                                                                                                                                                                                                                                                                                                                   
                                                                                                                                                                                                                                                                                                                                                                                                                      
Note the key here being the space character (not empty quotes) for the specified text