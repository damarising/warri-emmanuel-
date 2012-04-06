This is a question every eventual contributor faces at some point. The answer depends on your skills and interests.                                                                 
                                                                                                                                                                                    
== Source code and bug fixes contributions ==                                                                                                                                       
                                                                                                                                                                                    
'''Frank wrote [http://lists.osgeo.org/pipermail/mapserver-dev/2010-March/009816.html a very detailed answer to that question] from a developer's point of view on the -dev list:'''
                                                                                                                                                                                    
Until you are on the MapServer commit list you won't actually                                                                                                                       
be able to commit the changes yourself.  So you could take the approach of:                                                                                                         
                                                                                                                                                                                    
1) Build MapServer trunk from svn, with debug info.                                                                                                                                 
                                                                                                                                                                                    
2) Scanning the open bug list for something that catches your fancy and                                                                                                             
seems in within your competency:                                                                                                                                                    
                                                                                                                                                                                    
http://trac.osgeo.org/mapserver/query?status=%21closed&type=defect&order=id&desc=1                                                                                                  
                                                                                                                                                                                    
3) Make an attempt to reproduce the problem.  If this is going to take a                                                                                                            
fair amount of work, you might want to mention in the ticket that you are                                                                                                           
trying to investigate the problem.  If you succeed or fail to reproduce the                                                                                                         
problem make notes in the ticket, possibly including a simplified set of                                                                                                            
steps to reproduce the problem.                                                                                                                                                     
                                                                                                                                                                                    
4) If you have reproduced the problem you can start looking for a solution.                                                                                                         
How to accomplish this is pretty broad, but                                                                                                                                         
                                                                                                                                                                                    
5) If you come up with a solution, attach it to the ticket as a patch.                                                                                                              
You can create a patch using "svn diff > mybug.patch".  Describe the                                                                                                                
nature of the bug and the rationale of your change if appropriate.                                                                                                                  
                                                                                                                                                                                    
6) Now you are dependent on a committer to review and apply your patch.                                                                                                             
You may find that the originally assignee (like Steve by default) picks up                                                                                                          
on your patch and proceeds.  But if there is no action after a couple days                                                                                                          
you might want to prod the ticket holder by email, ask on IRC for help                                                                                                              
with the patch or here on mapserver-dev.                                                                                                                                            
                                                                                                                                                                                    
7) Repeat steps 1-6 until people become annoyed enough applying your                                                                                                                
patches that they ask if you would be willing to become a committer so                                                                                                              
you can do it yourself.                                                                                                                                                             
                                                                                                                                                                                    
8) Also consider preparing new regression tests for the "msautotest"                                                                                                                
test suite when you fix something.                                                                                                                                                  
                                                                                                                                                                                    
--                                                                                                                                                                                  
                                                                                                                                                                                    
In some cases you may not be able to fix a problem, but if you can                                                                                                                  
reproduce it and narrow it down to a test case that is highly forcused                                                                                                              
and easy for another developer to pick up, then you have already                                                                                                                    
accomplished something useful.                                                                                                                                                      
                                                                                                                                                                                    
In the above, I have assumed you wouldn't assign the ticket to yourself.                                                                                                            
But once you are a bit more confident, you can start to "seize" tickets                                                                                                             
that don't seem to be getting any love.  I'd suggest noting politely that                                                                                                           
you are interested in looking into the issue unless the original owner                                                                                                              
was wanting to take care of it.                                                                                                                                                     
                                                                                                                                                                                    
--                                                                                                                                                                                  
                                                                                                                                                                                    
See also the [http://mapserver.org/development/index.html MapServer Development] section of the website.                                                                            
                                                                                                                                                                                    
== Documentation contributions ==                                                                                                                                                   
                                                                                                                                                                                    
See the [http://mapserver.org/development/documentation.html Documentation Development Guide].                                                                                      
                                                                                                                                                                                    

