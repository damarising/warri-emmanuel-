# Adding Color Ramps In MapServer

Mapserver has the ability to display raster data using color ramps which is useful for displaying Hillshades, Digital Elevation Models, and other quantitative data.

CLASS
 
  EXPRESSION ([pixel] >= 1524 AND [pixel] < 1621) 

  STYLE 

    COLORRANGE 179 240 240 255 255 180   #color start RGB and end RGB

    DATARANGE 1524 1621  #the data range is normally set to the class expression range

    RANGEITEM "pixel" # pixel is the elevation value in a DEM

  END #STYLE

END #end class


## DEM Example

CLASS

  EXPRESSION ([pixel] >= 1524 AND [pixel] < 1621)

  STYLE

    COLORRANGE 175 240 233 255 255 179

    DATARANGE 1524 1621

    RANGEITEM "pixel"

  END #STYLE

END #end class

CLASS

  EXPRESSION ([pixel] >= 1621 AND [pixel] < 1670)

  STYLE

    COLORRANGE 255 255 190 0 128 70 

    DATARANGE 1621 1670

    RANGEITEM "pixel"

  END #STYLE

END #end class

CLASS

  EXPRESSION ([pixel] >= 1670 AND [pixel] < 1713)

  STYLE

    COLORRANGE 0 128 70 250 180 0 

    DATARANGE 1670 1713

    RANGEITEM "pixel"

  END #STYLE

END #end class

CLASS

  EXPRESSION ([pixel] >= 1713 AND [pixel] < 1753)

  STYLE

    COLORRANGE 250 180 0 130 0 0 

    DATARANGE 1713 1753

    RANGEITEM "pixel"

    END #STYLE

  END #end class

CLASS

  EXPRESSION ([pixel] >= 1753 AND [pixel] < 1803)

  STYLE

    COLORRANGE 130 0 0 110 60 20 

    DATARANGE 1753 1803

    RANGEITEM "pixel"

  END #STYLE

END #end class

CLASS

  EXPRESSION ([pixel] >= 1803 AND [pixel] < 1870)

  STYLE

    COLORRANGE 110 60 20 180 180 180 

    DATARANGE 1803 1870

    RANGEITEM "pixel"

  END #STYLE

END #end class

CLASS

  EXPRESSION ([pixel] >= 1870)

  STYLE

    COLORRANGE 180 180 180 255 255 252 

    DATARANGE 1870 1997

    RANGEITEM "pixel"

  END #STYLE

END #end class

## Hillshade Example

CLASS

  STYLE

    COLORRANGE 0 0 0 252 252 252

    DATARANGE 153 204 #note: the data range here does not have to be the same as the range in the source data.

    RANGEITEM "pixel"

  END #Style

END #Class

# Note, alternatively you can GDAL to build a color-relief image

## This information is provided from Even Rouault


An alternate way is to use the gdaldem utility from GDAL with the color-relief 
mode :  http://www.gdal.org/gdaldem.html

As a bonus, you can avoid generating a full new raster, by outputing to a VRT 
file (an XML file) that will only contain the LUT to map the elevations to 
colors.

gdaldem color-relief n43.dt0 dem.pct n43_pct.vrt -of vrt

with dem.pct :
3500   white
2500   235:220:175
50%   190 185 135
700    240 250 150
0      50  180  50
nv     0   0   0   0 

generates n43_pct.vrt  :

<VRTDataset rasterXSize="121" rasterYSize="121">
  <SRS>GEOGCS[&quot;WGS 84&quot;,DATUM[&quot;WGS_1984&quot;,SPHEROID[&quot;WGS 
84&quot;,6378137,298.257223563]],PRIMEM[&quot;Greenwich&quot;,0],UNIT[&quot;degree&quot;,0.0174532925199433],AUTHORITY[&quot;EPSG&quot;,&quot;4326&quot;]]</SRS>
  <GeoTransform> -80.00416666666666, 0.008333333333333333, 0, 
44.00416666666666, 0, -0.008333333333333333</GeoTransform>
  <VRTRasterBand dataType="Byte" band="1">
    <ColorInterp>Red</ColorInterp>
    <ComplexSource>
      <SourceFilename relativeToVRT="1">n43.dt0</SourceFilename>
      <SourceBand>1</SourceBand>
      <SourceProperties RasterXSize="121" RasterYSize="121" DataType="Int16" 
BlockXSize="1" BlockYSize="121"/>
      <SrcRect xOff="0" yOff="0" xSize="121" ySize="121"/>
      <DstRect xOff="0" yOff="0" xSize="121" ySize="121"/>
      <LUT>-32767:0,0:50,267.5:190,700:240,2500:235,3500:255</LUT>
    </ComplexSource>
  </VRTRasterBand>
  <VRTRasterBand dataType="Byte" band="2">
    <ColorInterp>Green</ColorInterp>
    <ComplexSource>
      <SourceFilename relativeToVRT="1">n43.dt0</SourceFilename>
      <SourceBand>1</SourceBand>
      <SourceProperties RasterXSize="121" RasterYSize="121" DataType="Int16" 
BlockXSize="1" BlockYSize="121"/>
      <SrcRect xOff="0" yOff="0" xSize="121" ySize="121"/>
      <DstRect xOff="0" yOff="0" xSize="121" ySize="121"/>
      <LUT>-32767:0,0:180,267.5:185,700:250,2500:220,3500:255</LUT>
    </ComplexSource>
  </VRTRasterBand>
  <VRTRasterBand dataType="Byte" band="3">
    <ColorInterp>Blue</ColorInterp>
    <ComplexSource>
      <SourceFilename relativeToVRT="1">n43.dt0</SourceFilename>
      <SourceBand>1</SourceBand>
      <SourceProperties RasterXSize="121" RasterYSize="121" DataType="Int16" 
BlockXSize="1" BlockYSize="121"/>
      <SrcRect xOff="0" yOff="0" xSize="121" ySize="121"/>
      <DstRect xOff="0" yOff="0" xSize="121" ySize="121"/>
      <LUT>-32767:0,0:50,267.5:135,700:150,2500:175,3500:255</LUT>
    </ComplexSource>
  </VRTRasterBand>
</VRTDataset>

that you can use as a raster name in your mapfile.


