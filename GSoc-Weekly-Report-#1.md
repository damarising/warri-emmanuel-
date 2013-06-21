                                                                                                                            
                                                            François Desjarlais                                                     
                                                               June 13, 2013 
###
This report will be larger than the one after it. Exceptionnally, I've been working on my project for the last 4 weeks because I have schedule conflicts. There is a lot of work done and I feel things are going the right way. I'll just summarize these last weeks.

###Week 1:
I had no experience in software devellopment so for these two weeks i had to familiarize myself with everything MapServer was using. On the very first week, i did all the tutorials with OpenLayers and MapServer. I learned a bit of http, and xml. I also learned how to use MapServer. For my project, i also had to learn how UTFGrid was working. For this, i used MapBox's TileMill and TileStream. There wasnt any difficulties during this first week. All i had to do was read documentation and do some tutorials.

###Week 2:
On the second week, I started by learning JavaScript and using the JavaScript console in my web browser. I also got my dev environment ready. I made my own web apps using the UTFGrid generated with TileMill and OpenLayers. I also made my own mapfile. This week was a bit more challenging yet it was still easy. Like the last week i mostly read docs. The JavaScript gave me a bit of problems tho. Most of my problems came from the Same Origin Policy.

###Week 3:
This is where the real project started. First I went to Chicoutimi and started to work in the MapGears office. I met my mentor Daniel, started to use MapServer in debug mode and look at how the code in it is getting the work done. By the end of the week, I could use an http request and ask MapServer to send me a UTFGrid that i hardcoded in the saveImage function. Of all the week I worked this one was the most challenging. MapServer source files are huge and I had difficulties understanding where some of the data came from and where they were generated in the code. I also got difficulties understanding how the AGG2 librairy worked.

###Week 4:
On this 4th week, I feel comfortable with the code. I have a good understanding of how MapServer generate maps. I'm currently working on my UTFGrid driver. I found how to set AGG to not use antialising. I also added support for UTFGrid with a WMS service in the OpenLayers librairy. I'm currently generating grid of the right size. I'm using the rasterizer buffer to get my data My two weeks at MapGears office in Chicoutimi are over and I have a good headstart. This week difficulties were mostly related to JavaScript and AGG librairy. 

###Plan:
Next week I plan to use color bytes to access my data. I'll likely only access country name on the beginning but as thing will go on I'll add more to it. This is merely the base and by the end it should work just like MapBox UTFGrid does.
