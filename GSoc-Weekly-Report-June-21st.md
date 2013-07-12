                                                                                                                            
                                                            François Desjarlais                                                     
                                                               June 21, 2013 
##Report #1
This report will be longer than the one after it. Exceptionnally, I've been working on my project for the last 4 weeks because I have schedule conflicts. There is a lot of work done and I feel things are going the right way. I'll just summarize these last weeks.

###Week 1:
I had no experience in software devellopment so for the first week I had to familiarize myself with all the tools MapServer is using. During this very first week, I did all the tutorials with OpenLayers and MapServer. I learned a bit of http, and xml. I also learned how to use MapServer. For my project, I also had to learn how UTFGrid is working. To learn this, I used MapBox's TileMill and TileStream. There wasn't any difficulties during this first week. All I had to do was read documentation and do some tutorials.

###Week 2:
On the second week, I started by learning JavaScript and using the JavaScript console in my web browser. I also got my dev environment ready. I made my own web apps using the UTFGrid generated with TileMill and OpenLayers. I also made my own mapfile. This week was a bit more challenging. Yet it was still easy. Like the last week I mostly read doc. The JavaScript gave me a bit of problems tho. Most of my problems were due to the same origin policy from http.

###Week 3:
This is where the real project started. First, I went to Chicoutimi and started to work in the MapGears' office. I met my mentor Daniel, started to use MapServer in debug mode and look at how the code in it is getting the work done. By the end of the week, I could use an http request and ask MapServer to send me a UTFGrid that I had hardcoded in the saveImage function. Of all the weeks I worked, this one was the most challenging. MapServer source files are huge and I had difficulties understanding where some of the data came from and where they were generated in the code. I also got difficulties understanding how the AGG2 library worked.

###Week 4:
On this 4th week, I feel comfortable with the code. I have a good understanding of how MapServer generate maps. I'm currently working on my UTFGrid driver. I found how to set AGG to not use anti-aliasing. I also added support for UTFGrid with a WMS service in the OpenLayers library. I'm currently generating grid of the right size. I'm using the rasterizer buffer to access my data and tag my shapes in the grid. My two weeks at MapGears office in Chicoutimi are over and I have a good headstart. This week difficulties were mostly related to JavaScript and AGG library. AGG doc is kind of cryptic and trying to see how the code works in debug is simply dazing.

###Plan:
Next week I plan to use color bytes to access my data. I'll likely only access country name on the beginning but as thing will go on I'll add more to it. This is merely the base and by the end it should work just like MapBox UTFGrid does.

[This GSoC's wiki main page](GSoC-UTF-Grid-implementation)