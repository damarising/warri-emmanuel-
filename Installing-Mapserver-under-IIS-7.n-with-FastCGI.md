Notes shared on the mapserver-users mailing list by donald.kerr_at_dkerr.co.uk and copied here for the benefit of all.

Also covers setting up Classic ASP.

# Configuring IIS

1. Click Start, Control Panel, Programs, Turn Windows features on or off.
2. Select _Internet Information Services_.
3. Select Web Management Tools then select _IIS Management Console_.
4.  Select World Wide Web Services
5. In Application development Features, select _ASP, CGI, ISAPI Extensions, ISAPI Filters and Server-Side Includes_.
6. In Common HTTP Features, select Default Document, HTTP Errors, HTTP Redirection and Static Content.
7. In Health and Diagnostics, select HTTP Logging and Request Monitor.
8. In Performance Features, select Static Content Compression.
9. In Security, select Request Filtering.
10. Navigate to http://localhost – An IIS welcome page should be shown confirming set up so far.
11. Run inetmgr.
12. Highlight _Default Web Site_ under Sites the right-click, Manage Web Site then click Advanced Settings. Change the physical path to one specific to your needs, example: d:\mymapsite\web\.
13. Double-click ASP in the centre panel of IIS Manager and ensure the following are set:
    * _Enable Parent Paths_
15. Click on Application Pools in the left hand panel of IIS Manager, click DefaultAppPool in the centre panel then click advanced settings in the right hand panel. Ensure that Enable 32-Bit Applications is set to True.
16. Create the following virtual directories in IIS:
    * cgi-bin                    d:\mapserver\cgi-bin (This will probably be MS4W cgi-bin)
    * mapserver               d:\mapserver\wwwroot (You might not require this)
19. Click on Default Web Site then double-click Handler Mappings in the centre panel.
20. In the Actions Pane, click Add Module Mapping then set the following:
    * Request path:         *.exe
    * Module:                  FastCgiModule
    * Executable:             d:\mapserver\cgi-bin\mapserv.exe
    * Name:                     Mapserver via FastCGI
25. Click on Request Restrictions and set the following:
    * Mapping Tab: Check “Invoke handler … “, then set for “File”.
    * Verbs Tab: “One of the following …”, enter “GET,HEAD,POST”.
    * Access Tab: Check “Execute”.
29. Whilst the “MapServer via FastCGI” handler mapping is highlighted, click on Edit feature permissions in the right hand panel then ensure that read, script and execute are checked.

## For 64 bit MapServer:
 
1. MapServer x64
2. Download x64 version of MapServer from http://www.gisinternals.com/sdk/ Alternatively, there are the 32 bit MapServer files from MS4W (http://ms4w.com) - please contact [Gateway Geomatics](mailto:info@gatewaygeomatics.com), the developers of MS4W, directly if you need custom x64 builds. 
3. Delete all existing files in d:\mapserver\cgi-bin\. 
4. From the above download, copy all dlls from /bin/ to d:\MapServer\cgi-bin\.
5. Copy Mapserv.exe from /bin/ms/apps/ to d:\mapserver\cgi-bin
