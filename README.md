<?php
// mySQL Veritabanı Bağlantısı
$link = mysql_connect('localhost' , 'root' , '');
mysql_select_db('ats_motor' , $link) or die(mysql_error());
mysql_query("set names 'utf8'");
 
// Sayfalama
$satir_sayisi = mysql_query("SELECT COUNT(Kimlik) FROM ats_motor");
$satir_sayisi = mysql_result( $satir_sayisi , 0);
$sayfa          = isset($_GET['aranan']) && intval($_GET['aranan']) > 0 ? $_GET['aranan'] : 1;
$limit          = 200;
$sayfa_sayisi = ceil( $satir_sayisi / $limit );
$sayfa          = ( $sayfa > $sayfa_sayisi ? 1 : $sayfa );
$goster          = ( $sayfa * $limit ) - $limit;
 
// Sorgu
$query          = mysql_query("SELECT * FROM ats_motor limit $goster,$limit");
$dizi          = array();
while( $row = mysql_fetch_array($query))
{
    $dizi[] = $row;
}
?>
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd">
<html xmlns="http://www.w3.org/1999/xhtml" xml:lang="en">
<head>
    <meta http-equiv="Content-Type" content="text/html;charset=UTF-8" />
    <title>Php ile sayfalama yapımı</title>
    <style type="text/css">
    body {font: 14px/24px Arial}
    ul , li {margin: 0; padding: 0; list-style-type : none}
    ul.liste {border-bottom: 1px solid #ccc; padding-bottom: 10px}
    ul.sayfalama {margin-top: 10px;}
    ul.sayfalama li {margin-right: 10px; font-size: 11px; display: inline-block; padding: 2px 8px; background-color: #efefef; border: 1px solid #ccc; border-radius: 3px}
    ul.sayfalama li a {text-decoration: none; color: #424242; display: block}
    ul.sayfalama li:hover , ul.sayfalama li.aktif {background-color: #424242;}
    ul.sayfalama li:hover a , ul.sayfalama li.aktif a , ul.sayfalama li.aktif { color: #fff}
    </style>
</head>
<body>
    <div id="icerik">
        <ul class="liste">
        <?php foreach( $dizi as $row ) : ?>
            <li><?php echo $row['Marka_Model']; ?></li>
        <?php endforeach; ?>
        </ul>
          
        <ul class="sayfalama">
        <?php
        if( $sayfa > 1 )
        {
            echo '<li><a href="?sayfa=1">İlk</a></li>';
            echo '<li><a href="?sayfa='.($sayfa - 1).'">Önceki</a></li>';
        }
        else
        {
            echo '<li class="aktif">İlk</li>';
            echo '<li class="aktif">Önceki</li>';
        }
          
        for( $i = $sayfa - 3; $i < $sayfa + 4; $i++ )
        {
            if( $i > 0 && $i <= $sayfa_sayisi )
            {
                echo '<li class="'.($i == $sayfa ? 'aktif' : 'pasif').'"><a href="?sayfa='.$i.'">'.$i.'</a></li>';
            }
        }
          
        if( $sayfa != $sayfa_sayisi )
        {
            echo '<li><a href="?sayfa='.($sayfa + 1).'">Sonraki</a></li>';
            echo '<li><a href="?sayfa=1">Son</a></li>';
        }
        else
        {
            echo '<li class="aktif">Sonraki</li>';
            echo '<li><a href="?sayfa='.($sayfa).'">Son</a></li>';
        }
        ?>
        </ul>
    </div>
</body>
</html>
