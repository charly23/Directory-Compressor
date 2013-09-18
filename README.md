Directory-Compressor
====================

function folderToZip($folder, &$zipFile, $subfolder = null) {
    if ($zipFile == null) {
        // no resource given, exit
        return false;
    }
    // we check if $folder has a slash at its end, if not, we append one
    $folder .= end(str_split($folder)) == "/" ? "" : "/";
    $subfolder .= end(str_split($subfolder)) == "/" ? "" : "/";
    
    if( $folder == true ){

            // we start by going through all files in $folder
            $handle = opendir($folder);
            while ($f = readdir($handle)) {
                if ($f != "." && $f != "..") {
                    if (is_file($folder . $f)) {

                        if ($subfolder != null):
                            $zipFile->addFile($folder . $f, $subfolder . $f);
                        else:
                            $zipFile->addFile($folder . $f);
                        endif;
                        
                    } elseif (is_dir($folder . $f)) {
                        $zipFile->addEmptyDir($f);
                        folderToZip($folder . $f, $zipFile, $f);
                    }
                }
            }
            
            return mktime() . ' - ' . ' Comparessed';
      } 
}

$z = new ZipArchive();
$z->open( mktime().'.zip', ZIPARCHIVE::CREATE);
echo folderToZip( 'dir/test', $z);
$z->close();
