* **Time**: 19th of January 2017 at [15.00 UTC](http://www.timeanddate.com/worldclock/fixedtime.html?year=2017&month=01&day=19&hour=15&min=0&sec=0%2016.00UTC)
* **Location**: #mapserver channel on irc.freenode.net (connect directly in your browser via [webchat](https://webchat.freenode.net/?channels=mapserver))
* **Agenda**
    * handle "secure" tickets in old Trac instance ([ticket](https://trac.osgeo.org/mapserver/ticket/4275))
    * propose [RFC 117](http://mapserver.org/development/rfc/ms-rfc-117.html) (PHP MapScript through SWIG) for acceptance
    * next Release?
    * Daytona Beach [Code Sprint](https://wiki.osgeo.org/wiki/Daytona_Beach_Code_Sprint_2017) updates
    * *add your items here*
    * Set next meeting date

* **Log**: http://irclogs.geoapt.com/mapserver/%23mapserver.2017-01-19.log

* **Summary**
  * Attending:
    * Bruno Friedmann
    * Seth Girvin
    * Steve Lime
    * Jeff McKenna
    * Even Rouault
    * Steven Woodbridge
  * Discussion
    * Handling "secure" tickets in old Trac instance
      * motion to simply delete old 3 tickets
      * also discussion on dropping the entire Trac instance
        * however the old Trac wiki pages need to be checked to make sure they exist on the Github wiki (already done by JeffM, needs another set of eyes)
    * PHP 7 MapScript through SWIG, and RFC 117
      * plan is to give more time for review of RFC, and then call for vote end of next week
      * concerns by SteveW on saying he needs Daniel's input
    * Plan for 7.2 release
      * BrunoF requested Python3 + Python2 to work nicely, for packagers
      * SteveL working on vector tiles, with Thomas' code
      * JeffM working on PHP 7, and goals of making PHP and Python versions work nicely
    * Update on the next OSGeo code sprint
      * http://wiki.osgeo.org/wiki/Daytona_Beach_Code_Sprint_2017
      * SethG asked for plans to contribute remotely
    * Agreement on next meeting date: 2017-03-02 [15:00 UTC](http://www.timeanddate.com/worldclock/fixedtime.html?year=2017&month=03&day=02&hour=15&min=0&sec=0%2016.00UTC)

* **Action Items**
  * JeffM: tackle old Trac tickets issue.  Update: the 3 problem Trac tickets are now deleted.
  * JeffM: bring RFC 117 to vote to the PSC through email next week
  * all: prepare for 7.2 release