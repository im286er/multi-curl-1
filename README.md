����
-----

����curl_multi_*ϵ�к���+�ص��ﵽ���̲߳ɼ���

����
----
PHP 5.3 +

��װ
----
composer require luojixinhao/multi-curl:*

��ϵ
--------
Email: lx_2010@qq.com<br>


## ʾ��

```php
<?php

use luojixinhao\mCurl;

$mc = new multiCurl();
$mc->add('http://proxyip.9window.com/api/getproxyiplist/10');
$mc->add('http://im.qq.com/album/');
$re = $mc->run(4); //��ʹ�ûص���ֱ�ӷ��ؽ��
print_r($re);
```

```php
<?php

use luojixinhao\mCurl;

$conf = array(
	'maxTry' => 2, //ʧ�ܳ��Դ���
	'maxConcur' => 5, //��󲢷���
);
$mc = new multiCurl($conf);
$mc->add('http://proxyip.9window.com/api/getproxyiplist/10', array(
	CURLOPT_USERAGENT => 'test',
), array(
	'arg1' => 'testArg'
), function($url, $content, $args, $header, $errorno, $error){
	//�ɹ�ʱ�Ļص�
	print_r(func_get_args());
}, function($url, $content, $args, $header, $errorno, $error){
	//ʧ��ʱ�Ļص�
	print_r(func_get_args());
});
$mc->run();
```

```php
<?php

use luojixinhao\mCurl;

$mc = new multiCurl();
//�������ٶ�̬���
$mc->run(function() use ($mc) {
	$mc->add('http://proxyip.9window.com/api/getproxyiplist/10', null, null,
		function($url, $content, $args, $header, $errorno, $error) {
		//�ɹ�ʱ�Ļص�
		print_r(func_get_args());
	}, function($url, $content, $args, $header, $errorno, $error) {
		//ʧ��ʱ�Ļص�
		print_r(func_get_args());
	});
});
```

```php
<?php

use luojixinhao\mCurl;

$mc = new multiCurl();
//��ͣ�ɼ�ĳ����ַ�����ɼ�����10�κ�ֹͣ
$mc->run(function() use ($mc) {
	$mc->add('http://im.qq.com/album/', null, null,
		function($url, $content, $args, $header, $errorno, $error) {
		//�ɹ�ʱ�Ļص�
		print_r(func_get_args());
	}, function($url, $content, $args, $header, $errorno, $error) {
		//ʧ��ʱ�Ļص�
		print_r(func_get_args());
	});

	if ($mc->getInfos('finishNum') >= 10) {
		return false;
	}
	return true;
});
```