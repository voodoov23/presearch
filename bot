<?php
// Created by YarzCode
// Everyone hate me, I don't have good friends

echo "PreSearch.org BOT - Created by YarzCode\n\n";

echo "Enter Email: ";
$email = trim(fgets(STDIN));
echo "Enter Password: \e[0;30m";
$password = trim(fgets(STDIN));
echo "\e[0m";

$login = login($email, $password);

if(is_array($login))
{
	echo "[!] Login Successfully.\n\n";
	$i=1;
	while(true)
	{
		$search = search($login);
		if($search == "OK")
		{
			$warnain = "\e[1;32m";
		} else {
			$warnain = "\e[1;31m";
		}
		echo $i.". Search ".$warnain."".$search."\e[0m - ";
		sleep(2);
		echo "Balance \e[1;32m".balanceNow($login[1])."\e[0m\n";
		$i++;
		sleep(3);
	}
} else {
	echo "Invalid Email/Password.";
}

function login($email, $password)
{
	$feed = yarzCurl('https://presearch.org/login');
	if(strpos($feed[1], '<input type="hidden" name="_token"'))
	{
		preg_match_all('/Set-Cookie: (.*?);/', $feed[0], $cookies);
		$kuki='';
		foreach($cookies[1] as $cookie)
		{
			$kuki .= $cookie."; ";
		}
		preg_match('/<input type="hidden" name="_token" value="(.*?)"/', $feed[1], $token);
		$header = array();
		$header[] = 'Cookie: '.$kuki;
		$header[] = 'Content-Type: application/x-www-form-urlencoded; charset=UTF-8';
		$header[] = 'Accept-language: en-US,en;q=0.9';
		$header[] = 'User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/72.0.3626.121 Safari/537.36';
		$header[] = 'X-Requested-With: XMLHttpRequest';

		$login = yarzCurl('https://presearch.org/api/auth/login', '_token='.$token[1].'&login_form=1&email='.str_replace('@', '%40', $email).'&password='.$password, false, $header);

		if(json_decode($login[1])->status == 'OK')
		{
		    preg_match_all('/Set-Cookie: (.*?);/', $login[0], $cookies);
		    $kuki='';
		    foreach($cookies[1] as $cookie)
		    {
		    	$kuki .= $cookie."; ";
		    }
		    return array($token[1], $kuki);
		} else {
			return "Error on Login.";
		}
	}
}

function search($data)
{
	$body = 'term='.rand(11111,99999).'&provider_id=98&_token='.$data[0];
    $header = array();
    $header[] = 'Cookie: '.$data[1];
    $header[] = 'Content-Type: application/x-www-form-urlencoded; charset=UTF-8';
    $header[] = 'Accept-language: en-US,en;q=0.9';
    $header[] = 'User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/72.0.3626.121 Safari/537.36';
    $header[] = 'X-Requested-With: XMLHttpRequest';	

    $search = yarzCurl('https://presearch.org/presearch', $body, false, $header);
    if(strpos($search[0], '302 Found'))
    {
    	return "OK";
    } else {
    	return "ERROR";
    }
}

function balanceNow($cookie)
{
    $header = array();
    $header[] = 'Cookie: '.$cookie;	
    $header[] = 'Accept-language: en-US,en;q=0.9';
    $header[] = 'User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/72.0.3626.121 Safari/537.36';

    $balance = yarzCurl('https://presearch.org/', false,false, $header);
    preg_match('/<span class="mobile-tour-balance"><span class="number">(.*?)<\/span>/', $balance[1], $result);

    if(isset($result[1]))
    {
    	return $result[1]." PRE";
    } else {
    	return "ERROR";
    }
}

function yarzCurl($url, $fields=false, $cookie=false, $httpheader=false, $proxy=false, $encoding=false, $timeout=false, $useragent=false, $put=false)
{ 
	$ch = curl_init();
	curl_setopt($ch, CURLOPT_URL, $url);
	curl_setopt($ch, CURLOPT_RETURNTRANSFER, 1);
	curl_setopt($ch, CURLOPT_FOLLOWLOCATION, 1);
	curl_setopt($ch, CURLOPT_SSL_VERIFYPEER, false);
	curl_setopt($ch, CURLOPT_HEADER, 1);
	if($useragent !== false)
	{
		curl_setopt($ch, CURLOPT_USERAGENT, $useragent);
	}
	if($put !== false)
	{
		curl_setopt($ch, CURLOPT_CUSTOMREQUEST, "PUT");
	}
	if($fields !== false)
	{ 
		curl_setopt($ch, CURLOPT_POST, true);
		curl_setopt($ch, CURLOPT_POSTFIELDS, $fields);
	}
	if($encoding !== false)
	{ 
		curl_setopt($ch, CURLOPT_ENCODING, 'gzip, deflate');
	}
	if($cookie !== false)
	{ 
		curl_setopt($ch, CURLOPT_COOKIEFILE, $cookie);
		curl_setopt($ch, CURLOPT_COOKIEJAR, $cookie);
	}
	if($httpheader !== false)
	{ 
		curl_setopt($ch, CURLOPT_HTTPHEADER, $httpheader);
	}
	if($proxy !== false)
	{ 
		curl_setopt($ch, CURLOPT_PROXY, $proxy);
		curl_setopt($ch, CURLOPT_PROXYTYPE, CURLPROXY_SOCKS5);
	}
	if($timeout !== false)
	{ 
       curl_setopt($ch, CURLOPT_CONNECTTIMEOUT, 0); 
       curl_setopt($ch, CURLOPT_TIMEOUT, 6); //timeout in seconds		
	}
	$response = curl_exec($ch);
	$header = substr($response, 0, curl_getinfo($ch, CURLINFO_HEADER_SIZE));
	$body = substr($response, curl_getinfo($ch, CURLINFO_HEADER_SIZE));
	curl_close($ch);
	return array($header, $body);
}
