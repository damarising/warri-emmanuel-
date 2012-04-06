Much of this code is hard coded specifically for this application, but hopefully bits and pieces can serve as a useful example.                                         
                                                                                                                                                                        
Code sample created by David Bitner, frequent lurker on !MapServer mailing list and IRC channels.                                                                       
                                                                                                                                                                        
{{{                                                                                                                                                                     
<?php                                                                                                                                                                   
set_time_limit(60000);                                                                                                                                                  
ini_set('max_execution_time',0);                                                                                                                                        
dl("php_mapscript.so");                                                                                                                                                 
$mapfile=$_REQUEST['map'];                                                                                                                                              
$map = ms_newMapObj($mapfile);                                                                                                                                          
$map->selectOutputFormat('jpeg'); //fpdf seems to play better with jpeg                                                                                                 
                                                                                                                                                                        
//get map extent                                                                                                                                                        
if ($_REQUEST['mapext']) {                                                                                                                                              
       $mapext = $_REQUEST['mapext'];                                                                                                                                   
       $extent = explode(" ", $mapext);                                                                                                                                 
       $map->setExtent($extent[0], $extent[1],$extent[2], $extent[3]);                                                                                                  
}                                                                                                                                                                       
                                                                                                                                                                        
//get list of layers and cycle through to turn them on                                                                                                                  
if (isset($_REQUEST['layers'])) {                                                                                                                                       
       $layerlist = rtrim($_REQUEST['layers']);                                                                                                                         
       $layers = explode(" ", $layerlist);                                                                                                                              
       $numLayers = $map->numlayers;                                                                                                                                    
       for ($i = 0; $i < $numLayers; $i++) {                                                                                                                            
               $activeLayer = $map->getlayer($i);                                                                                                                       
                           if ($activeLayer->status != 2) $activeLayer->set("status", 0);                                                                               
       }                                                                                                                                                                
       foreach ($layers as $l) {                                                                                                                                        
               $activeLayer = $map->getlayerbyname($l);                                                                                                                 
               $activeLayer->set("status", 1);                                                                                                                          
       }                                                                                                                                                                
}                                                                                                                                                                       
$ft=$map->getlayerbyname('FlightTracks');                                                                                                                               
$ftstat=$ft->status;                                                                                                                                                    
// this is used to allow cgi substitutions in data strings                                                                                                              
for ($i = 0; $i < $map->numlayers; $i++) {                                                                                                                              
        $activeLayer = $map->getlayer($i);                                                                                                                              
        $datastring=$activeLayer->data;                                                                                                                                 
        if (preg_match_all("/%([\w].+?)%/",$datastring,$matches,PREG_SET_ORDER)>0){                                                                                     
                foreach ($matches as $match){                                                                                                                           
                        $search=$match[0];                                                                                                                              
                        $replace=$match[1];                                                                                                                             
                        if (isset($_REQUEST[$replace])){                                                                                                                
                                $replaceval=$_REQUEST[$replace];                                                                                                        
                                $datastring=str_replace($search,$replaceval,$datastring);                                                                               
                                $activeLayer->set("data",$datastring);                                                                                                  
                        } else {                                                                                                                                        
                                if (!($activeLayer->name == 'FlightTracks' && isset($_REQUEST['sDate']) && isset($_REQUEST['eDate']))) $activeLayer->set("status", 0);  
                        }                                                                                                                                               
                }                                                                                                                                                       
                                                                                                                                                                        
        }                                                                                                                                                               
}                                                                                                                                                                       
$headerstring='';                                                                                                                                                       
                                                                                                                                                                        
//conditionals used to only display certain messages when certain layers are turned on                                                                                  
$ft=$map->getlayerbyname('FlightTracks');                                                                                                                               
$ftstat=$ft->status;                                                                                                                                                    
$add=$map->getlayerbyname('house');                                                                                                                                     
$addstat=$add->status;                                                                                                                                                  
if ($ftstat==1) $headerstring=$headerstring.'Time Period: '.$_REQUEST['sDate'].'-'.$_REQUEST['eDate'];                                                                  
if ($addstat==1) $headerstring=$headerstring.' Address Shown: '.$_REQUEST['add'];                                                                                       
                                                                                                                                                                        
                                                                                                                                                                        
$resolution=isset($_REQUEST['resolution'])?$_REQUEST['resolution']:150; //allows to set resolution or defaults to 150dpi                                                
$mapfile_resolution=$map->resolution;                                                                                                                                   
$scalefactor=$resolution/$mapfile_resolution; //how much everything needs to be multiplied by                                                                           
$map->set('resolution',$resolution); // set new resolution for min/max scale renderings                                                                                 
                                                                                                                                                                        
                                                                                                                                                                        
// loop through layers to extract class images and names and to resize styles and truetype symbols                                                                      
$numLayers = $map->numlayers;                                                                                                                                           
$classimages=array();                                                                                                                                                   
$classnames=array();                                                                                                                                                    
for ($i=0; $i < $numLayers; $i++) {                                                                                                                                     
        $layer = $map->getlayer($i);                                                                                                                                    
        if ($layer->status == 1 ){                                                                                                                                      
        for ($j=0; $j < $layer->numclasses; $j++) {                                                                                                                     
                $class = $layer->getClass($j);                                                                                                                          
                $label = $class->label;                                                                                                                                 
                if ($layer->type !=4){                                                                                                                                  
                        $classimageobj=$class->createLegendIcon(60,60);                                                                                                 
                        $classimages[]=$classimageobj->saveWebImage();                                                                                                  
                        $classnames[]=$class->name;                                                                                                                     
                }                                                                                                                                                       
                if ($label->mindistance>1){                                                                                                                             
                        $md=$label->mindistance;                                                                                                                        
                        $newmd=$md*$scalefactor;                                                                                                                        
                        $label->set('mindistance',$newmd);                                                                                                              
                }                                                                                                                                                       
                if ($label->type == 'TRUETYPE') {                                                                                                                       
                        $labelsize=$label->size;                                                                                                                        
                        $newlabelsize=$labelsize*$scalefactor;                                                                                                          
                        $label->set('size',$newlabelsize);                                                                                                              
                }                                                                                                                                                       
                for ($k=0; $k < $class->numstyles; $k++) {                                                                                                              
                        $style = $class->getStyle($k);                                                                                                                  
                        $style->set("size", $style->size * $scalefactor);                                                                                               
                        $style->set("offsetx", $style->offsetx * $scalefactor);                                                                                         
                        $style->set("offsety", $style->offsety * $scalefactor);                                                                                         
                }                                                                                                                                                       
        }                                                                                                                                                               
        }                                                                                                                                                               
}                                                                                                                                                                       
$nrows=ceil(count($classnames)/4); // make 4 columns to display legend                                                                                                  
$legheight=$nrows*.25; // determine height of legend                                                                                                                    
$mapheight=9.3-$legheight; // determine area left to draw map                                                                                                           
$legstart=$mapheight+1.05; // determine where to start the legend                                                                                                       
                                                                                                                                                                        
$map->set("width", 7.6*$resolution);                                                                                                                                    
$map->set("height", $resolution*$mapheight); // make map rectangle fit page                                                                                             
//echo $mapheight*$resolution;                                                                                                                                          
//draw map                                                                                                                                                              
$imgMap = $map->draw();                                                                                                                                                 
$urlMap = $imgMap->saveWebImage();                                                                                                                                      
                                                                                                                                                                        
require('fpdf.php');                                                                                                                                                    
class PDF extends FPDF                                                                                                                                                  
{                                                                                                                                                                       
//Page header                                                                                                                                                           
function Header()                                                                                                                                                       
{                                                                                                                                                                       
                                                                                                                                                                        
}                                                                                                                                                                       
                                                                                                                                                                        
//Page footer                                                                                                                                                           
function Footer()                                                                                                                                                       
{                                                                                                                                                                       
    $this->SetY(-1);                                                                                                                                                    
    $this->SetFont('Arial','I',7);                                                                                                                                      
}                                                                                                                                                                       
function draw_legend($textarray,$imgarray,$startx,$starty,$nr){ // function draws the 4 column table                                                                    
        $this->SetFont('Times','',12);                                                                                                                                  
        $y=$starty-.25;                                                                                                                                                 
        $x=$startx;                                                                                                                                                     
        $first=true;                                                                                                                                                    
                                                                                                                                                                        
        for ($i=0;$i<count($textarray);$i++){                                                                                                                           
                if ($i==$nr or $i==2*$nr or $i==3*$nr) {                                                                                                                
                        $y=$starty-.25;                                                                                                                                 
                        $x=$x+1.9;                                                                                                                                      
                        $first=false;                                                                                                                                   
                }                                                                                                                                                       
                $y=$y+.25;                                                                                                                                              
                $imgurl=$imgarray[$i];                                                                                                                                  
                $text=$textarray[$i];                                                                                                                                   
                $this->Image($imgurl,$x,$y,.20,.20);                                                                                                                    
                $this->Text($x+.25,$y+.15,$text);                                                                                                                       
        }                                                                                                                                                               
}                                                                                                                                                                       
}                                                                                                                                                                       
                                                                                                                                                                        
//Instanciation of inherited class                                                                                                                                      
$pdf=new PDF('P','in','letter');                                                                                                                                        
                                                                                                                                                                        
$pdf->SetDisplayMode('fullpage');                                                                                                                                       
$pdf->SetMargins(.25,.25);                                                                                                                                              
                                                                                                                                                                        
$pdf->AddPage();                                                                                                                                                        
                                                                                                                                                                        
                                                                                                                                                                        
$pdf->SetFont('Arial','B',14);                                                                                                                                          
$pdf->Cell(0,.50,'MACNOISE Interactive Map',0,1,'C');                                                                                                                   
$pdf->SetFont('Arial','',10);                                                                                                                                           
$pdf->Cell(0,.02,$headerstring,0,1,'C');                                                                                                                                
                                                                                                                                                                        
                                                                                                                                                                        
$pdf->SetLineWidth(.1);                                                                                                                                                 
$pdf->SetDrawColor(0,0,150);                                                                                                                                            
$pdf->Rect(.25,.25,8,10.5);                                                                                                                                             
                                                                                                                                                                        
$pdf->Image($urlMap,.45,.9,7.6,$mapheight);                                                                                                                             
$pdf->Image("/var/apache/htdocs/graphics/MACnorth1.png",.45,.9,1,1);                                                                                                    
                                                                                                                                                                        
$pdf->SetLineWidth(.01);                                                                                                                                                
$pdf->SetDrawColor(0,0,200);                                                                                                                                            
$pdf->Rect(.40,.85,7.7,$mapheight+.1);                                                                                                                                  
$pdf->Rect(.40,$legstart,7.7,$legheight+.1);                                                                                                                            
                                                                                                                                                                        
$date=date('M j, Y g:i');                                                                                                                                               
$datemessage="Map created by http://maps.macnoise.com/interactive/ on $date. This information is to be used for reference purposes only.";                              
$disclaimer="The Metropolitan Airports Commission does not guarantee accuracy of the material contained herein and is not responsible for misuse or misinterpretation.";
$pdf->SetFont('Arial','I',7);                                                                                                                                           
$pdf->Text(.45,10.55,$datemessage);                                                                                                                                     
$pdf->Text(.45,10.65,$disclaimer);                                                                                                                                      
$pdf->SetTitle('MACNoise Interactive Map');                                                                                                                             
$pdf->draw_legend($classnames,$classimages,.45,$legstart+.05,$nrows);                                                                                                   
                                                                                                                                                                        
$pdf->Output();                                                                                                                                                         
//$pdf->Output('macnoise.pdf','D');                                                                                                                                     
?>                                                                                                                                                                      
}}}                                                                                                                                                                     
                                                                                                                                                                        
                                                                                                                                                                        
----                                                                                                                                                                    
back to [wiki:PHPMapScript
