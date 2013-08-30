                                                                                                                            
                                                            François Desjarlais                                                     
                                                              August 30, 2013 
##Report #10

Reports #10 goes from August 17th to August 30th. MapServer with UTFGrid driver can be found on my [Git](https://github.com/fdesj). You can find the docs and working example [here](http://msgsoc.mapgears.com).

###Week 12-13:

Last week, I didn't make a report because I was sick and there wasn't enough work done. This week, I worked on finishing all the tasks I had to do for this project. I received feedback and made all the necessary changes. I also made a tests suite for my UTFGrid driver in the msautotest. There was not any kind of difficulties this week and things went pretty well. Since some of the work isn't completely done, I plan to work on the weekend to finish it before the school restarts.

###Plan:

This plan states what are the big chunks of the UTFGrid driver that are left.

1.  Do a final code review with my mentor to see if everything is fine.
2.  Add a renderTruetypeSymbols. Due to the upcoming changes to Truetype rendering, I didn't make one and adding it will allow UTFGrid to render every symbols types and labels.
3.  Make a JSON escaping function for UTFGrid rendering.
4.  An OpenLayers patch is required for the MapServer UTFGrid output to work. It is available at https://github.com/fdesj/openlayers in the utfgridwms branch, and the corresponding OL ticket is at https://github.com/openlayers/openlayers/pull/1076.  We will need to followup with the OpenLayers developpers to make sure it gets integrated.

###Conclusion

It was a great summer and learned a whole lot of stuff. I would like to thanks the MapServer devs and the Mapgears' team for their great support during this whole project.

[Main page](GSoC-UTF-Grid-implementation)