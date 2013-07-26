                                                                                                                             
                                                            François Desjarlais                                                     
                                                               July 19, 2013 

#### Introduction

For the 2013 Google Summer of Code (GSoC), Francois Desjarlais will be working on the addition of support for UTF Grid in [Mapserver](http://mapserver.org/). This page is to collect information about the project, design documents, weekly reports, etc.

Mentors: Daniel Morissette, Thomas Bonfort

The goal of my project is to enable MapServer to produce UTFGrid output. This will be implemented as a new output format that will generate UTFGrid (JSON) as output from a normal map draw operation, that is, via CGI mode=map or WMS GetMap requests. The UTFGrid encoding scheme encodes interactivity data for a tile in a space efficient manner. It is designed to be used in browsers, e.g. for displaying tooltips when hovering over certain features of a map tile. A RFC has been prepared about this, it is available in the link below and should be updated as the project is going.

Edit:
July 19th, driver open testing should be soon.

#### Weekly reports

1.  [Weekly reports #1: Weeks 1,2,3 and 4](GSoc-Weekly-Report-June-21st)
2.  [Weekly reports #2: First iteration](GSoC-Weekly-Report-June-28th)
3.  [Weekly reports #3: Getting the datas](GSoC-Weekly-Report-July-5th)
4.  [Weekly reports #4: Code review](GSoC-Weekly-Report-July-12th)
5.  [Weekly reports #5: UTF-8 encoding](GSoC-Weekly-Report-July-19th)
6.  [Weekly reports #6: Symbol rendering](GSoC-Weekly-Report-July-26th)

#### Useful links

1.  [MS RFC 93 UTF Grid support](http://mapserver.org/development/rfc/ms-rfc-93.html)
2.  Code available on my Git: [Code](https://github.com/fdesj/mapserver/tree/utfgridgsoc)
3.  UTGrid WMS example: [Working example](http://msgsoc.mapgears.com/projet_utfgrid/testhtmlmapserver.html)

Example looks cool, works great but has many bugs probably due to OpenLayers.