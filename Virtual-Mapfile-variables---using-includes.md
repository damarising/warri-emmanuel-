# Background
I had have a stupidly large and complicated mapfile.  I started tweaking the colours and styles on the map and found it very painful - as I have the color tag for creeks repeated about 20 times.<br>
To to teak the creek colour just a bit I needed to search and replace these values - painful.
As will many other styles.

# Solution
There is no way of setting simple variables in the plain mapfile (yet). So this work around uses 'INCLUDE' to point to a specific file, that contains the 'variable' value.
```
    CLASS
        EXPRESSION 'creek'
        STYLE
            COLOR   INCLUDE "includes/creek_colour.txt"
        END
   END
```
Then in "includes/creek_colour.txt" you just have "0 0 200"<br>
(paths are relative to the mapfile)<br>
But the file can include more.  It may include the entire style or layer etc.<br>
<br>
Seems to work well for me, I am not sure of the performance hit, that is for someone smarter them me to work out.
<br>
# Making it easier
I have written a little php script to create and edit these include files.  It uses ajax and is very fast for making these tweaks.  So I can spend more time making the map look pretty and less time waiting for files to save. NOTE there is no security on this file.  Please feel free to improve this script. No warranty - just have fun with it
```php
<?php
//deal with updates
if($_POST[editorId] != "" & $_POST[value] != ""){
ECHO(write_to_file($_POST[editorId], $_POST[value]));
//if submit is ajax then die, else continue happily
if($_POST[submit] != 'Create'){die;}
}

echo('
<script src="http://ajax.googleapis.com/ajax/libs/prototype/1.7.0.0/prototype.js" type="text/javascript"></script>
<script src="https://ajax.googleapis.com/ajax/libs/scriptaculous/1.9.0/scriptaculous.js" type="text/javascript"></script>
');

$files = get_file_list();
sort($files);
Echo("<table border='1'>
<tr>
<td>File</td>
<td>Contents</td>
</tr>");

foreach($files as $file){
echo("<tr><td>$file</td>");
echo("<td><p id='$file'>".get_file_contents($file)."</p>
<script type='text/javascript'>
new Ajax.InPlaceEditor('$file', '', {rows:20,cols:150});
</script>
</td></tr>");
}
echo("</table>");

Echo("<br><br><br><form action='' method='post'>
New file:<br><input type='text' name='editorId' /><br>
<textarea name='value' rows='10' cols='30'></textarea> 
<br>
<input type='submit' name='submit' value='Create' />
</form>");


//############ Write content to a file
function write_to_file($file, $content){
$fh = fopen($file, 'w') or die("can't open file");
fwrite($fh,$content);
fclose($fh);

return(get_file_contents($file));
}

//##############  get files and put then in $files array
function get_file_list(){
if ($handle = opendir('.')) {
    while (false !== ($entry = readdir($handle))) {
        if (substr($entry,-4) == ".txt" ) {
            $files[] = $entry;
        }
    }
    closedir($handle);
}
return($files);
}


//#############  get contents of a file
function get_file_contents($file){
$fh = fopen($file, 'r');
$data = fread($fh, filesize($file));
fclose($fh);
return($data);
}
?>
```
