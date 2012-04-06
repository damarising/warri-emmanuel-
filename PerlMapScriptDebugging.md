== Submitted by Aaron D. Hunt <hunt@zedxinc.com> ==                                                                
{{{                                                                                                                
    $ENV{MS_ERRORFILE} = "/var/log/mapserver.log"; # map engine error log                                          
}}}                                                                                                                
    near the top of the perl mapscript code... before                                                              
    you initialize your map object.                                                                                
    This is an environment variable: MS_ERRORFILE                                                                  
    for the shell, or execution instance which catches                                                             
    all of the errors/warning that occur within mapserver                                                          
    and sends them to the filespec defined. This one is                                                            
    the *most* useful log for debugging mapfile errors,                                                            
    index problems, and mapserver internals, etc.                                                                  
{{{                                                                                                                
    WEB                                                                                                            
    log "/var/log/mapserver_access.log                                                                             
    END                                                                                                            
}}}                                                                                                                
    in the mapfile. you could probably setup that variable in                                                      
    mapscript as well, but I have always put it in the mapfile.                                                    
    It records a set of variables similar to the                                                                   
    apache access log for each mapserver execution.                                                                
                                                                                                                   
== Submitted by Lowell.Filak <lfilak@medinaco.org> ==                                                              
{{{                                                                                                                
    #!/usr/bin/perl -w                                                                                             
                                                                                                                   
    -w posts all Perl warnings to the httpd log file,                                                              
    usually named error_log in /var/log/httpd or /etc/httpd/logs.                                                  
}}}                                                                                                                
== Submitted, in part, by Frank Warmerdam <warmerdam@pobox.com> ==                                                 
{{{                                                                                                                
    /sbin/ldconfig -v                                                                                              
}}}                                                                                                                
    will give information concerning the shared libraries present                                                  
    on your system.                                                                                                
{{{                                                                                                                
    echo $LD_LIBRARY_PATH                                                                                          
}}}                                                                                                                
    Will give information on the library path traversed when                                                       
    linking.                                                                                                       
{{{                                                                                                                
    locate <library_name_ie._gd.h>                                                                                 
}}}                                                                                                                
    Will give location(s) of the specified filename.                                                               
                                                                                                                   
    Does the above information show a mixture of include files                                                     
    (different revisions of the same library).                                                                     
                                                                                                                   
== Submitted by Daniel Morissette <dmorissette@mapgears.com> ==                                                    
{{{                                                                                                                
    ldd mapserv                                                                                                    
}}}                                                                                                                
    Look for the path that is listed.                                                                              
    Then make sure this path is part of your runtime library path,                                                 
    for this you have two options, assuming you're running Linux:                                                  
                                                                                                                   
    1- add the path to /etc/ld.so.conf and run 'ldconfig' as root                                                  
                                                                                                                   
    or                                                                                                             
                                                                                                                   
    2- Use !SetEnv LD_LIBRARY_PATH to specify this path in your                                                    
    apache httpd.conf                                                                                              
                                                                                                                   
If when running ./mapserv you receive the error message:                                                           
{{{                                                                                                                
    mapserv:error while loading libraries:libproj.so.0 cannot open shared object file : No such file or directory. 
}}}                                                                                                                
    Run '/sbin/ldconfig -v | grep libproj'.                                                                        
                                                                                                                   
        Which should return something similar to "libproj.so.0"                                                    
        If not try reinstalling proj.4                                                                             
                                                                                                                   
== Submitted by Daniel Morissette <dmorissette@mapgears.com> ==                                                    
                                                                                                                   
    If when running ./mapserv you recieve the error message:                                                       
{{{                                                                                                                
    error while loading shared libraries :/usr/libwwwappp.so.0 undefined symbol: HTZlib_inflate                    
}}}                                                                                                                
    Check /usr/lib and /usr/local/lib for a copy of w3c-libwww.                                                    
    If both have a copy then remove the copy in /usr/lib.                                                          
    The cleanest way to remove, is to 'rpm -q -a | grep w3c-libwww' & do 'rpm -e w3c-libwww-<version_number>'.     
                                                                                                                   
If you end up with a "core" file from an error and have the GNU debugger installed:                                
{{{                                                                                                                
    gdb                                                                                                            
    target core ./core                                                                                             
    backtrace                                                                                                      
}}
