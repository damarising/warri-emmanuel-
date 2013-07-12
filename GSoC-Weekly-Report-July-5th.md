                                                                                                                            
                                                            François Desjarlais                                                     
                                                               July 5, 2013 
##Report #3
Reports #3 goes from June 29th to July 5th.

###Week 6:
This week is probably the toughest since the beginning of the project. Like I planned last week, I changed my code so that UTFGrid directly call AGG2 driver instead of using a copy of it. I made this possible by adding a fake output in my driver. With this output, it is possible to change the rendering parameter easily. By calling AGG directly it was no longer necessary to use C++ so I changed all my code to C. Also, like I planned, I added they keywords UTFRESOLUTION, UTFITEM and UTFDATA to the mapfile syntax. UTFRESOLUTION lets you set the grid resolution for each characters. Default resolution is 4 per char. UTFDATA is used to set which data returns with the UTFGrid. It uses classObj->text syntax so it's easy to set. Finally, I'm working on making shapes data available for my driver.

###Plan:
Next week I plan to bring the datas in my driver. Once It will be done, I will do another iteration, review my code and update my example.

[My wiki main page](GSoC-UTF-Grid-implementation)