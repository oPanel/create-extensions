[![tr](https://img.shields.io/badge/lang-tr-red.svg)](README-TR.md)
[![en](https://img.shields.io/badge/lang-en-yellow.svg)](README.md)

# oPanel Yeni Modül Oluşturma/Ekleme

## Kullanılabilecek Özellikler Listesi:
* WoM Menü Ekleme
* oPanel Menü Ekleme
* Güncelleme sistemine modül silinmemesi için dışlama ekleme
* Apache için httpd.conf yapılandırması ekleme
* Toplu modül dosyaları için özel dizin ekleme
* Modül çoklu dil özelliği için dil dosyasına ek parametreler eklemek
* WoM ve oPanel sayfa formatını kullanabilme

## Özellikler Kullanım Detayı:
* WoM Menü Eklemek için "/usr/local/opanel/www/admin/" dizini altına "extensions_ModuleName.php" şeklinde php dosyası oluşturmalısınız.
* oPanel Menü Eklemek için "/usr/local/opanel/www/user/" dizini altına "extensions_ModuleName.php" şeklinde php dosyası oluşturmalısınız.
* ```$update=$oPanel->setini('update');``` ile alınacak ```$update['panel']['noupdirs']``` bilgisini alarak mevcut liste altına "\n/usr/local/opanel/www/admin/extensions_ModuleName.php" gibi mevcut geçerli bir dosya veya klasör ekleyerek ```$oPanel->setini('update','panel/noupdirs',$update['panel']['noupdirs']);``` şeklinde 3.parametrede veri gönderilip kalıcı olarak kayıt edebilirsiniz. Bu sayede panel güncellemelerde silinmesini engellemelisiniz.
* WoM için özel Apache yapılandırması eklemek için /usr/local/opanel/conf/httpd-admin-ModuleName.conf biçiminde, oPanel için özel Apache yapılandırması eklemek için /usr/local/opanel/conf/httpd-user-ModuleName.conf  biçiminde Apache parametrelerini içeren bir dosya oluşturun, ardından Apache'yi sıfırlayın veya "apachectl graceful" kullanın.
* Modül için WoM ve oPanel URL erişimi sonuna /ext/ModuleName/ ekleyip çoklu dosya dizin oluşturmak için çoklu modül dosyalarını /usr/local/opanel/www/ext/ altında "ModuleName" gibi modül adında bir dizin oluşturarak http://WoM&oPanel/ext/ModuleName/ url adresi ile erişebilirsiniz. (Private logo/css/inc files ex.)
* Modül içinde zabit kelimeler yazabilir, özel dil yapınızı oluşturabilir veya oPanel dil sistemini kullanabilirsiniz. oPanel dil sistemini kullanacaksanız ```<?=$oPanel->lang('ext_ModuleName_title')?>``` gibi dil bağlantılı parametreler ekleyebilirsiniz, eklenen parametreler sayfaya ilk girişte girilen dil dosyasına otomatik olarak yazılır veya önceden siz ekleyebilirsiniz. "/usr/local/opanel/www/conf/lang/" altında yer alan "en.php" gibi dil dosyaları altına php array olarak ekleme ve düzenleme yapabilirsiniz.
* WoM ve oPanel sayfa formatını kullanmak için alttaki default sayfa kaynak kodlarını kullanabilirsiniz.

## WoM Sayfa Formatı:
```php
<?php
require_once("../config/config.php");
$Page	= [
	'title_key'	=> substr(basename(__FILE__),0,-4),
	'css_key'	=> substr(basename(__FILE__),0,-4),
	'js_key'	=> substr(basename(__FILE__),0,-4),
];
if(isset($_POST['action'])){
	if(...){
		$Page['alert'][]=['success'=>"İşlem Başarılı"];
	}else{
		$Page['alert'][]=['danger'=>$oPanel->lang('Error').": ..."];
	}
}
require_once('header.php');
?>

...

<?php
require_once('footer.php');
```


## oPanel Sayfa Formatı:
```php
<?php
require_once("../config/config.php");
$Page	= [
	'title_key'	=> substr(basename(__FILE__),0,-4),
	'css_key'	=> substr(basename(__FILE__),0,-4),
	'js_key'	=> substr(basename(__FILE__),0,-4),
	'navs'		=> ['/'=>'index','/'.$UrlCut[0]=>$UrlCut[0],'/'.implode('/',$UrlCut)=>implode('_',$UrlCut)]
];
if(isset($_POST['action'])){
	if(...){
		$Page['alert'][]=['success'=>"İşlem Başarılı"];
	}else{
		$Page['alert'][]=['danger'=>$oPanel->lang('Error').": ..."];
	}
}
require_once('header.php');
?>

...

<?php
require_once('footer.php');
```
