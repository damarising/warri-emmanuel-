                                                                                                                            
                                                            François Desjarlais                                                     
                                                               July 19, 2013 
##Report #5

Reports #5 goes from July 13th to July 19th. There was several modifications to my code and it is now up to date.[Example](http://msgsoc.mapgears.com/projet_utfgrid/testhtmlmapserver.html)

###Week 7:

This week, I changed my UTFGrid driver so it now uses its own instance of AGG with a uint32 pixels. It took me a lot less time than expected. I also fixed my OpenLayers panning problem. Instead of the 2 days I thought it would take it took me about 1 hour. Then, I added the renderLine function to my UTFGrid driver. Finally, I spent most of the week time working on the UTF-8 encoding. The difficulties I met this week all came from the UTF-8 encoding. I tried several ways of doing it and none give me the same results on my computer and on the Ubuntu server.

###Plan:

1.  Get the UTF-8 encoding working on both machines. (1 day to 2 days)
2.  Add the renderSymbols to my driver. (1 day)
3.  Add the duplicates and labels formatOption for the UTFGrid output. (1/2 day)
4.  Change the image size of the output from inside the driver. (rest of week)

[Main page](GSoC-UTF-Grid-implementation)