                                                                                                                            
                                                            François Desjarlais                                                     
                                                               June 28, 2013 
##Report #2
This is my second report as I am working on adding UTFGrid support for MapServer. There should be a working example, just look on my main page Useful links tab.

###Week 5:
This week, we're back at MapGears' office in Quebec City. I've been working on my UTFGrid driver and now I can render an UTFGrid. However, my grid can only return the shapeID of countries. It works with getMap and WMS requests. I also added some keywords to the lexer and a bit of code to make them possible to use(still a WIP). Most of the difficulties this week came from how to get map data into the UTFGrid driver. I still have to figure it out. Also, since i'm no longer in Chicoutimi offices, communication with my mentor is a bit harder.

###Plan:
This first grid driver is only the first iteration for my GSoC project. Now I have to rework it. First, I plan to directly call AGG2 driver from UTFGrid driver instead of using a copy it. I also want to add keywords for grid resolution. Then, I need to implement the UTFITEM and UTFDATA.

[Main page](GSoC-UTF-Grid-implementation)