== 0. Introduction ==                                                                                                                                                                                                                                                                                                                                                                                         
                                                                                                                                                                                                                                                                                                                                                                                                              
=== Why ===                                                                                                                                                                                                                                                                                                                                                                                                   
Allowing strangers to interact with your computer on the Internet exposes you to the risk that someone will use that opportunity inappropriately. The more ways you allow people to interact with your computer, (all other things being equal) the greater this risk.                                                                                                                                        
                                                                                                                                                                                                                                                                                                                                                                                                              
No matter how carefully an application is vetted and tested, there will always be the possibility that a mistake has been made. For example, in 2002, vulnerabilities were discovered in Open SSL and BIND, SNMP and SOAP, established protocols and applications that are widely deployed and well understood.                                                                                               
                                                                                                                                                                                                                                                                                                                                                                                                              
=== How ===                                                                                                                                                                                                                                                                                                                                                                                                   
So what do we do? We want to let legitamate users to be able to use our Mapserver, but we can't ever be sure if we are going to get hacked or not. In this circumstance, the best thing we can do is to understand and manage these risks to reduce the likelyhood and severity.                                                                                                                              
                                                                                                                                                                                                                                                                                                                                                                                                              
== 1. Methods of Attack ==                                                                                                                                                                                                                                                                                                                                                                                    
Before we can minimise the risk of someone breaking our Mapserver, we need to have some understanding how Mapserver (or any other web application) can be broken. Briefly, methods of attack are:                                                                                                                                                                                                             
                                                                                                                                                                                                                                                                                                                                                                                                              
=== 1.1 Denial of Service ===                                                                                                                                                                                                                                                                                                                                                                                 
It is not neccesary to break into a computer to stop other people from using it. Flooding the computer with many spurious requests may prevent other, legitamate users from making requests. Because it may take many megabytes of data and considerable CPU time to render a map, Mapserver can be vulnerable to this form of attack.                                                                        
                                                                                                                                                                                                                                                                                                                                                                                                              
=== 1.2 Implementation Errors ===                                                                                                                                                                                                                                                                                                                                                                             
With multiple services and applications on a computer, there is a risk that applications can interact in un-intentional ways to compromise security.                                                                                                                                                                                                                                                          
                                                                                                                                                                                                                                                                                                                                                                                                              
=== 1.3 Programing Errors ===                                                                                                                                                                                                                                                                                                                                                                                 
If the program processing a user's request contains certain kinds of errors, a malicious user may be able to exploit such errors to stop the application from working (until it is re-started), or worse, break out of the application into the host system.                                                                                                                                                  
                                                                                                                                                                                                                                                                                                                                                                                                              
== 2. Securing your Mapserver ==                                                                                                                                                                                                                                                                                                                                                                              
Here are some documents that give more information on security:                                                                                                                                                                                                                                                                                                                                               
                                                                                                                                                                                                                                                                                                                                                                                                              
  * http://www.tldp.org/HOWTO/Security-HOWTO/index.html                                                                                                                                                                                                                                                                                                                                                       
  * http://www.tldp.org/HOWTO/Security-Quickstart-HOWTO/index.html                                                                                                                                                                                                                                                                                                                                            
                                                                                                                                                                                                                                                                                                                                                                                                              
=== 2.1 Quantifying risk ===                                                                                                                                                                                                                                                                                                                                                                                  
So just what sort of a risk does the Mapserver pose to your server? In looking at this question, I decided that it really depends on how you are running Mapserver. I have not justification for these assumptions, but they will do for now as a starting point.                                                                                                                                             
                                                                                                                                                                                                                                                                                                                                                                                                              
==== Higher: Native CGI ====                                                                                                                                                                                                                                                                                                                                                                                  
While we may tinker with the Mapserver source, there's probably only a handful of people in the world who fully understand it. While great care has been taken to ensure that there are no mistakes, it has not been subject to the same level of scrutiny compared to more widely used (and attacked) applications. Conversely, this relative obscurity probably makes Mapserver a less attractive target.   
                                                                                                                                                                                                                                                                                                                                                                                                              
==== Medium: CGI module ====                                                                                                                                                                                                                                                                                                                                                                                  
Running Mapserver as a module for some CGI scripting language allows you to limit how people are able to interact with Mapserver and to use any security features available with your favorite scripting language. IMHO, this is not as secure as something like PHP where many security decisions can be imposed centrally.                                                                                  
                                                                                                                                                                                                                                                                                                                                                                                                              
==== Lower: PHP module ====                                                                                                                                                                                                                                                                                                                                                                                   
PHP is a widely deployed, well understood server side scripting environment designed for web applications. As such, it includes many features that provide a base level of security and protect the server against poorly written applications. Because there is a large community and install base, the chances are that you will be able to secure any holes that may emerge before your server is attacked.
                                                                                                                                                                                                                                                                                                                                                                                                              
  * http://www.php.net/manual/en/security.php                                                                                                                                                                                                                                                                                                                                                                 
                                                                                                                                                                                                                                                                                                                                                                                                              
=== 2.2 Simplicity and clarity of design ===                                                                                                                                                                                                                                                                                                                                                                  
If you are going to secure your Mapserver, make sure you understand how people can interact with it. A clear design can aid in understanding the system and therefore possible means of compromising it. The more complicated something is, the greater the chance of mistakes.                                                                                                                               
                                                                                                                                                                                                                                                                                                                                                                                                              
=== 2.3 Detection and testing ===                                                                                                                                                                                                                                                                                                                                                                             
After good and thoughful design, you should still implement at least some of the inrusion detection measures available. This is a topic far beyond the scope of this document, but can include things like content checksumming, logging & log analysis and security audit tools. At least use something like nmap to make sure you haven't left something like "telnet" switched on.                         
                                                                                                                                                                                                                                                                                                                                                                                                              
=== 2.5 Stay up-to-date ===                                                                                                                                                                                                                                                                                                                                                                                   
Over time, security problems will be identified and rectified. This is of no help at all if you have forgotten about your server and no-one has applied those updates.                                                                                                                                                                                                                                        
                                                                                                                                                                                                                                                                                                                                                                                                              
== 3. Protecting your work ==                                                                                                                                                                                                                                                                                                                                                                                 
No hacker will be able to electronically break into your filing cabinet and erase a CD-R. A current backup will also protect you from other unfortunate incidents such as disk failures.                                                                                                                                                                                                                      
                                                                                                                                                                                                                                                                                                                                                                                                              
== Replication ==                                                                                                                                                                                                                                                                                                                                                                                             
Consider deplying your Mapserver as a tiered setup, replicating content to a production server from inside a firewall. This way your original code is never visible to the Internet and any compromises can be handled by wiping the box. This also lets you test updates to applications before deploying them.                                                                                              
                                                                                                                                                                                                                                                                                                                                                                                                              
== Compartmentalise ==                                                                                                                                                                                                                                                                                                                                                                                        
In line with simple design, compartmentalise to minimise the potential damage of any compromise.