Notes by Donald Kerr donald.kerr_at_dkerr.co.uk.

# Configuring FastCGI for Mapserver on IIS 5.n and 6.n (x86)

1. Download FastCGI Extension 1.5 for Internet Information Services 6.0 and 5.1 (x86) from http://www.microsoft.com/en-gb/download/details.aspx?id=11881
2. Install FastCGI running fcgisetup_1.5_x86_rtw.msi downloaded in 1. above.
3. Once the above install is completed, create a script mapping as follows:
* Open inetmgr.exe. 
* Expand the folder tree and right-click _Websites_ then click _Properties_. 
* Click the _Home Directory_ tab.  
* Click the _Configuration…_ button. 
* Click the _Add…_ button.
* In the Add/Edit Application Extension Mapping dialog box, click _Browse..._ then navigate to _fcgiext.dll_ that is located in _C:\WINDOWS\system32\inetsrv\_.
* Note that you will now have to click in the browse result input box in order for the OK button below to be enabled.
* In the Extension text box, enter _".exe"_ (no quotes). 
* Under Verbs, in the Limit to text box, enter _"GET,HEAD,POST"_ (no quotes and no spaces).
* Ensure that the Script engine and Verify that file exists check boxes are selected. 
* Click “OK” (three times to close all the open dialogs).
* Modify _fcigext.ini_ in _C:\WINDOWS\system32\inetsrv\_.
* Add an extension to application mapping “exe=EXE” to the [Types] section. 
* Add a [EXE] section with “ExePath=d:\mapserver\cgi-bin\mapserv.exe”

[Types]
exe=EXE

[EXE]
ExePath=d:\mapserver\cgi-bin\mapserv.exe