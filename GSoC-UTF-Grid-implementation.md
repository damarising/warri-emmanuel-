                                                                                                                             
                                                            François Desjarlais                                                     
                                                               June 13, 2013 

#### Introduction

For the 2013 Google Summer of Code (GSoC), Francois Desjarlais will be working on the addition of support for UTF Grid in [Mapserver](http://mapserver.org/). This page is to collect information about the project, design documents, weekly reports, etc.

Mentors: Daniel Morissette, Thomas Bonfort

The goal of my project is to enable MapServer to produce UTFGrid output. This will be implemented as a new output format that will generate UTFGrid (JSON) as output from a normal map draw operation, that is, via CGI mode=map or WMS GetMap requests. The UTFGrid encoding scheme encodes interactivity data for a tile in a space efficient manner. It is designed to be used in browsers, e.g. for displaying tooltips when hovering over certain features of a map tile. A RFC has been prepared about this, it is available in the link below and should be updated as the project is going.

[MS RFC 93 UTF Grid support](http://mapserver.org/development/rfc/ms-rfc-93.html)


#### Weekly reports

1.  [Weekly reports #1: Weeks 1,2,3 and 4](GSoc-Weekly-Report-June-21st)
2.  [Weekly reports #2: First iteration](GSoC-Weekly-Report-June-28th)

#### Code

Code available on my Git: [Code](https://github.com/fdesj/mapserver/tree/utfgridgsoc)
UTGrid WMS example: Coming soon!