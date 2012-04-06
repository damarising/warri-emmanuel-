  * all _esc tags, replace with a general string tag processor that handles various encodings              
                                                                                                           
{{{                                                                                                        
  [layers_esc] => [layers escape="url"]                                                                    
}}}                                                                                                        
                                                                                                           
  * layer/group state management tags, those that manage checkboxes and select lists (no replacement)      
  * zoom direction state management tags (no replacement)                                                  
  * zoom size state management tags (no replacement)                                                       
                                                                                                           
  * all extent _esc tags (this functionality is already in 5.6)                                            
                                                                                                           
{{{                                                                                                        
  [mapext_esc] => [mapext escape="url"]                                                                    
}}}                                                                                                        
                                                                                                           
  * all extent part tags (e.g. [minx] or [refminx]) (this functionality is already in 5.6)                 
                                                                                                           
{{{                                                                                                        
  [minx] => [mapext format="$minx"] or                                                                     
  [refmaxx] => [refext format="$maxx"]                                                                     
}}}                                                                                                        
                                                                                                           
  * all item_raw and _esc tags, use [item ...] tag instead, leave the shortcut [''itemname''] tags in place

