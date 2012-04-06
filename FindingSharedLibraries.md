== Question: ==                                                                                                                                                                     
                                                                                                                                                                                    
Why do I get the "mapserv: error while loading shared libraries: libXXX.so: cannot load shared object file: No such file or directory." message for a library when I run !MapServer?
                                                                                                                                                                                    
== Answer (from Armin Burger): ==                                                                                                                                                   
                                                                                                                                                                                    
   1. On Linux (and possibly some other Unix platforms) you can try it with adding the line /usr/local/lib (or whichever                                                            
      directory libXXX.so resides in) to the file /etc/ld.so.conf and then run 'ldconfig'. This will ensure that programs                                                           
      can find the library regardless of their environment, even !MapServer when run as a cgi.                                                                                      
   2. You could also use:                                                                                                                                                           
      {{{                                                                                                                                                                           
      setenv LD_LIBRARY_PATH /usr/lib:/usr/local/lib                                                                                                                                
      }}}                                                                                                                                                                           
      to add the library path. Or something similar                                                                                                                                 
      with export for bash. However, doing this for your account will not generally affect "mapserv" when run via CGI. To                                                           
      publish an environment variable for cgi programs via Apache you can usually put something like:                                                                               
      {{{                                                                                                                                                                           
      SetEnv LD_LIBRARY_PATH /usr/local/lib:/usr/local/pgsql/lib                                                                                                                    
      }}}                                                                                                                                                                           
      in your Apache configuration file.
