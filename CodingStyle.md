## Coding conventions in use with MapServer

MapServer uses mostly K&R code styling with the following changes:
* 2 space indenting (no tabs)
* case entries inside a switch are indented as so:
```c
  switch(layertype) {
    case MS_LAYER_POINT:
      if(ogrGeomPoints(hGeom, outshp) == -1) {
        nStatus = MS_FAILURE; // Error message already produced.
      }
      break;
    case MS_LAYER_LINE:
      if(ogrGeomLine(hGeom, outshp, MS_FALSE) == -1) {
        nStatus = MS_FAILURE; // Error message already produced.
      }
      break;
    default:
  }
```

For release 6.2, the MapServer team has decided that:
* the existing code will be re-formatted to respect the chosen coding style
* all future changes to the codebase should aim at respecting the chosen coding style


## Artistic Style (astyle)
                                                                                                                                                                                                                                            
[Artistic Style (astyle)](http://astyle.sourceforge.net/ ) is a source code
formatting tool, and allows for flexible definitions (spaces, padding, indents,
one liners, etc.) to automatically format source code for a given coding
convention.

Here is the invocation used to re-format the existing codebase:

```
$ astyle --style=kr --indent=spaces=2 -c --lineend=linux -S [files]
```

                                                                                                                                                                                                                                            