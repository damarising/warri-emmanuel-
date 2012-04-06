== Proposed Map File Syntax Example ==                                                                                                                  
                                                                                                                                                        

```                                                                                                                                                     
MAP                                                                                                                                                     
  STYLE                                                                                                                                                 
    # define a named style with global visibility                                                                                                       
    NAME "some_style"                                                                                                                                   
    ...                                                                                                                                                 
  END                                                                                                                                                   
  ...                                                                                                                                                   
  LAYER                                                                                                                                                 
    NAME "uses_a_style"                                                                                                                                 
    CLASS                                                                                                                                               
       # a name instead of a block means 'use the named style directly'                                                                                 
       STYLE "some_style"                                                                                                                               
       ...                                                                                                                                              
    END                                                                                                                                                 
  END                                                                                                                                                   
  LAYER                                                                                                                                                 
    NAME "extends_a_style"                                                                                                                              
    CLASS                                                                                                                                               
      STYLE                                                                                                                                             
        # inside a style block, pull in properties from another style                                                                                   
        # then make custom changes for this LAYER/CLASS                                                                                                 
        EXTENDS "some_style"                                                                                                                            
        ...                                                                                                                                             
      END                                                                                                                                               
    END                                                                                                                                                 
  END                                                                                                                                                   
END                                                                                                                                                     
```                                                                                                                                                     
                                                                                                                                                        
== Mapfile Extensions ==                                                                                                                                
                                                                                                                                                        
 * Enable `STYLE` blocks at the top `MAP` level                                                                                                         
   * Add a `NAME` directive to the `STYLE` block                                                                                                        
   * Top-level `STYLE` blocks ''must'' be named                                                                                                         
                                                                                                                                                        
 * Provide a "named style" declaration in the `LAYER` block in the form of `LAYER "name"` (in addition to the existing `LAYER ... END` syntax)          
   * The `LAYER` object gets instantiated with a ''copy'' of the named style from the top level                                                         
   * Referencing a non-existent `STYLE` by name throws an exception                                                                                     
                                                                                                                                                        
 * Add an `EXTENDS "style"` directive to the `STYLE` block                                                                                              
   * Block styles that inherit from a named style are instantied as a copy of the named style before other declarations in the `STYLE` block are applied
                                                                                                                                                        
== MapServer `StyleObj` Extensions ==                                                                                                                   
                                                                                                                                                        
 * Add a "name" property to styleObj                                                                                                                    
 * Add a "shared" property to styleObj                                                                                                                  
   * Exporting a map file would emit all styleObjs with style.shared set to true at the top level                                                       
   * Programmatic changes to shared styles *would* affect all classes using that style                                                                  
   * If you don't want that behavior, use cloneStyle/EXTENDS                                                                                            
 * Add a "cloneStyle" function to mapfile.c                                                                                                             
   * This would be the programmatic interface for `EXTENDS`                                                                                             
   * cloneStyle would presumably copy a top-level style and set the new object's `shared` property to false                                             
 * Look at INLINESYMBOLS(?) for exporting styles                                                                                                        
 * Possibly have the styles cacaded at render time rather than at mapfile parse time and work around the default issues                                 
                                                                                                                                                        
== MapScript Extensions ==                                                                                                                              
                                                                                                                                                        
 * ...?                                                                                                                                                 
 *
