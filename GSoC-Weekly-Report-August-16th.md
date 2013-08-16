                                                                                                                            
                                                            François Desjarlais                                                     
                                                              August 16, 2013 
##Report #9

Reports #9 goes from August 10th to August 16th. MapServer with UTFGrid driver can be found on my [Git](https://github.com/fdesj). You can find the docs and working example here: [Exemple](http://msgsoc.mapgears.com/projet_utfgrid/testhtmlmapserver.html)

###Week 11:

Now that my main task is done I need to move on the tiling function of the driver. I didn't have much feedback for my driver. Still, I got 2 bugs reported that I quickly fixed in about a day.
I also remade my lookup table so that it's less memory hungry now. Then, I fixed some other bugs that I found myself for the next 2 days. Finally, I started using MapCache to understand how it works. I need to understand how it works because I have to debug so I know how the code inside is.
This week there wasn't a lot of challenge. Most of the challenge came from using MapCache.

###Plan:

1.  Debug MapCache to see how it works. (2-3 days)
2.  Start making the tiling function for UTFGrid. (Rest of week)

[Main page](GSoC-UTF-Grid-implementation)