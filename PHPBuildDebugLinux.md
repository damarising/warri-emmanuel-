Stephen Woodbridge wrote:                                                                                                                                                                               
{{{                                                                                                                                                                                                     
 > I have finally gotten around to trying to debug the                                                                                                                                                  
 > PHP3/Mapscript? problem I was having. You said I should                                                                                                                                              
 > build PHP/Mapscript? as a static php CGI executable so I                                                                                                                                             
 > can run it in gdb.                                                                                                                                                                                   
}}}                                                                                                                                                                                                     
Actually, I meant compile PHP as a CGI executable instead of as an Apache DSO. And then you still compile PHP !MapScript the same way, as a php_mapscript.so file that will be loaded by PHP at runtime.
                                                                                                                                                                                                        
Once you're all setup, you can debug a PHP script that crashes using the 'php' CGI executable in gdb with:                                                                                              
{{{                                                                                                                                                                                                     
  $ gdb ./php                                                                                                                                                                                           
  ...                                                                                                                                                                                                   
  (gdb) run /path/to/test.php                                                                                                                                                                           
  ...                                                                                                                                                                                                   
}}}                                                                                                                                                                                                     
Here is the fun part: If you need to setup a breakpoint inside php_mapscript.so then you have to:                                                                                                       
                                                                                                                                                                                                        
 - First set a breakpoint in php_dl()                                                                                                                                                                   
 - start the program, when it reaches php_dl(), type "finish" to let                                                                                                                                    
   php_dl() finish its execution                                                                                                                                                                        
 - debugger will stop again at the end of php_dl(), after                                                                                                                                               
   php_mapscript.so is loaded... then you can set breakpoints anywhere                                                                                                                                  
   in php_mapscript.so and enjoy gdb!  :)                                                                                                                                                               
 - Make sure you disable all breakpoints before re-running the program                                                                                                                                  
   or gdb will keep complaining about them                                                                                                                                                              
                                                                                                                                                                                                        
There may be ways to automate this in gdb but I never looked into that any further... I welcome suggestions.                                                                                            
{{{                                                                                                                                                                                                     
 > All the documentation only talks about building it as a DSO module for                                                                                                                               
 > apache. Can you send me a link or have someone add it to Wiki? I think                                                                                                                               
 > this would be a great help to aid others that might want to work with                                                                                                                                
 > PHP3 and Mapscript.                                                                                                                                                                                  
 >                                                                                                                                                                                                      
}}}                                                                                                                                                                                                     
The answer to this question has been moved to a separate PHPMapScriptCGI page.                                                                                                                          
{{{                                                                                                                                                                                                     
 > Also have you guys posted documentation on how to build Mapscript for                                                                                                                                
 > PHP4 and !MapServer 3.5? on Wiki?                                                                                                                                                                    
 >                                                                                                                                                                                                      
}}}                                                                                                                                                                                                     
Nothing special to say there... !MapServer's configure takes care of it all on most platforms... here are some detailed steps (hopefully I didn't forget anything):                                     
                                                                                                                                                                                                        
  1. Compile and install PHP as a CGI as described above                                                                                                                                                
                                                                                                                                                                                                        
  2. Run configure in your mapserver dir. with the --with-php switch:                                                                                                                                   
                                                                                                                                                                                                        
     ./configure --with-php=/path/to/php-src-that-you-just-compiled ...                                                                                                                                 
                                                                                                                                                                                                        
  3. run 'make', that will automagically build mapserv, etc. ... and php_mapscript.so in mapserver/mapscript/php3                                                                                       
                                                                                                                                                                                                        
  4. Edit your php.ini and make sure extensions_dir is set to point to a valid location (e.g. /usr/local/lib/php4)                                                                                      
                                                                                                                                                                                                        
  5. Copy php_mapscript.so to your extensions_dir                                                                                                                                                       
                                                                                                                                                                                                        
  6. Make sure all libs used in your !MapServer build are included in your runtime library path. See the PHP !MapScript                                                                                 
     install FAQ about this common problem: http://www.mapserver.org/installation/php.html#faq-common-problems                                                                                          
                                                                                                                                                                                                        
  7. You're ready to use dl("php_mapscript.so"); to load and use PHP !MapScript in your PHP apps.                                                                                                       
                                                                                                                                                                                                        
See also the PHPMapScript-install-HOWTO for more info[http://www.cyberworldltd.co.uk/cases-for-apple-model-iphone-4.htm :] http://www.mapserver.org/installation/php.html                               
----                                                                                                                                                                                                    
Go back to: [wiki:PHPMapScript
