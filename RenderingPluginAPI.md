Symbolization Structures                                                                                        
                                                                                                                
Stroke:                                                                                                         

```                                                                                                             
typedef struct {                                                                                                
    double width; /* line width in pixels */                                                                    
                                                                                                                
    /* line pattern, e.g. dots, dashes, etc.. */                                                                
    int patternlength;                                                                                          
    double pattern[MS_MAXPATTERNLENGTH];                                                                        
                                                                                                                
    /* must not be NULL, must be a valid color */                                                               
    /* color.alpha must be used if supported by the renderer */                                                 
    colorObj color;                                                                                             
                                                                                                                
    int linecap; /* MS_CJC_TRIANGLE, MS_CJC_SQUARE, MS_CJC_ROUND, MS_CJC_BUTT */                                
    int linejoin; /* MS_CJC_BEVEL MS_CJC_ROUND MS_CJC_MITER */                                                  
    double linejoinmaxsize;                                                                                     
} strokeStyleObj;                                                                                               
```                                                                                                             
                                                                                                                
Fill:                                                                                                           

```                                                                                                             
typedef struct {                                                                                                
    /* must not be NULL, must be a valid color *                                                                
     * color.alpha must be used if supported by the renderer */                                                 
    colorObj color;                                                                                             
                                                                                                                
                                                                                                                
    /* if not null, use the passed tile (which is a pointer to a                                                
     * renderer specific structure) for tiling the polygon */                                                   
    void *tile;                                                                                                 
                                                                                                                
} fillStyleObj;                                                                                                 
```                                                                                                             
                                                                                                                
Booleans                                                                                                        
                                                                                                                
  * supports_imagecach                                                                                          
  * supports_pixel_buffer                                                                                       
  * supports_transparent_layers                                                                                 
                                                                                                                
Functions                                                                                                       
                                                                                                                
  * void startNewLayer(imageObj *img, double opacity)                                                           
  * void closeNewLayer(imageObj *img, double opacity)                                                           
                                                                                                                
  * imageObj *createImage(int width, int height, outputFormatObj *format, colorObj* bg)                         
  * int saveImage(imageObj *img, FILE *fp, outputFormatObj *format)                                             
  * void freeImage(imageObj *img)                                                                               
                                                                                                                
  * int transformShape(shapeObj *shape, rectObj extent, double cellsize)                                        
                                                                                                                
  * void renderLine(imageObj *img, shapeObj *p, strokeStyleObj *stroke)                                         
  * void renderPolygon(imageObj *img, shapeObj *p, colorObj *c)                                                 
  * void renderGlyphsLine(imageObj *img, labelPathObj *labelpath, labelStyleObj *style, char *text)             
  * void renderGlyphs(imageObj *img,double x, double y, labelStyleObj *style, char *text)                       
  * void renderEllipseSymbol(imageObj *img, double x, double y, symbolObj *symbol, symbolStyleObj *style)       
  * void renderVectorSymbol(imageObj *img, double x, double y, symbolObj *symbol, symbolStyleObj *style)        
  * void renderTruetypeSymbol(imageObj *img, double x, double y, symbolObj *symbol, symbolStyleObj *s)          
  * void renderPixmapSymbol(imageObj *img, double x, double y, symbolObj *symbol, symbolStyleObj *style)        
  * void renderTile(imageObj *img, imageObj *tile, double x, double y)                                          
  * void renderPolygonTiled(imageObj *img, shapeObj *p,  imageObj *tile)                                        
                                                                                                                
  * void getRasterBuffer(imageObj *img, rasterBufferObj *rb)                                                    
  * void mergeRasterBuffer(imageObj *img, rasterBufferObj *rb, double opacity, int dstX, int dstY)              
                                                                                                                
  * int getTruetypeTextBBox(imageObj *img,char *font, double size, char *text, rectObj *rect, double **advances)
                                                                                                                
  * void freeTile(imageObj *tile)                                                                               
  * void freeSymbol(symbolObj *s)                                                                               

