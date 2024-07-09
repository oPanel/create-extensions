[![en](https://img.shields.io/badge/lang-en-red.svg)](README.md)
[![tr](https://img.shields.io/badge/lang-tr-yellow.svg)](README-TR.md)

# oPanel Create/Add New Extensions

## Available Features List:
* Adding WoM Menu
* Adding oPanel Menu
* Adding an exclusion to prevent the module from being deleted in the update system
* Adding configuration to httpd.conf for Apache
* Adding a special directory for bulk module files
* Adding extra parameters to the language file for multi-language module support
* Utilizing WoM and oPanel page format

## Detailed Usage of Features:
* Adding WoM Menu: You need to create a PHP file named extensions_ModuleName.php under the directory /usr/local/opanel/www/admin/.
* Adding oPanel Menu: You need to create a PHP file named extensions_ModuleName.php under the directory /usr/local/opanel/www/user/.
* Preventing Module Deletion in Updates: Use ```$update=$oPanel->setini('update');``` to get the ```$update['panel']['noupdirs']``` information, and add a valid file or folder under this list such as \n/usr/local/opanel/www/admin/extensions_ModuleName.php. This should be done by sending data in the 3rd parameter like ```$oPanel->setini('update','panel/noupdirs',$update['panel']['noupdirs']);``` to save it permanently, preventing deletion during panel updates.
* Adding Apache Configuration: Create a file with Apache parameters in the format /usr/local/opanel/conf/httpd-admin-ModuleName.conf to add custom Apache configuration for WoM, in the format /usr/local/opanel/conf/httpd-user-ModuleName.conf to add custom Apache configuration for oPanel, then reset Apache or use "apachectl graceful"..
* Creating Directory for Bulk Module Files: For module URL access, append /ext/ModuleName/ to the WoM and oPanel URL and create a directory named ModuleName under /usr/local/opanel/www/ext/ Access it via http://WoM&oPanel/ext/ModuleName/ for private logo, CSS, or inc files.
* Multi-language Module Support: You can write fixed terms within the module, create your custom language structure, or use the oPanel language system. For using the oPanel language system, add language-related parameters like ```<?=$oPanel->lang('ext_ModuleName_title')?>``` These parameters will be automatically written to the language file entered on the first page load, or you can add them manually. Add and edit PHP arrays in language files like en.php under /usr/local/opanel/www/conf/lang/.
* Using WoM and oPanel Page Format: Utilize the default page source codes provided below for using WoM and oPanel page format.

## WoM Page Format:
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
		$Page['alert'][]=['success'=>"Action Success"];
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

## oPanel Page Format:
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
		$Page['alert'][]=['success'=>"Action Success"];
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

## WoM Apache Example Configuration
/usr/local/opanel/conf/httpd-admin-ModuleName.conf
```
Alias "/ModuleName" "/usr/local/ModuleName/web/"
<LocationMatch "^/ModuleName/.*\.php$">
        SetHandler "proxy:unix:/usr/local/opanel/tmp/php82-fpm.admin.sock|fcgi://."
</LocationMatch>
```

## oPanel Apache Example Configuration
/usr/local/opanel/conf/httpd-user-ModuleName.conf
```
Alias "/ModuleName" "/usr/local/ModuleName/web/"
<LocationMatch "^/ModuleName/.*\.php$">
        SetHandler "proxy:unix:/usr/local/opanel/tmp/php82-fpm.user.sock|fcgi://."
</LocationMatch>
```
> [!TIP]
> You can get the default php version in your configuration file to create it this way: ````$oPanel->config['php']['default']````
