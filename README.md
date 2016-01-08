#Google ReCAPTCHA ver.2 backend provider

[![SensioLabsInsight](https://insight.sensiolabs.com/projects/cbc2c849-3910-4316-bac2-9977c4eda736/big.png)](https://insight.sensiolabs.com/projects/cbc2c849-3910-4316-bac2-9977c4eda736)
[![Latest Stable Version](https://poser.pugx.org/dario_swain/re-captcha-library/v/stable.svg)](https://packagist.org/packages/dario_swain/re-captcha-library)
[![Build Status](https://travis-ci.org/DarioSwain/ReCaptchaLibrary.svg?branch=master)](https://travis-ci.org/DarioSwain/ReCaptchaLibrary)
[![Scrutinizer Code Quality](https://scrutinizer-ci.com/g/DarioSwain/ReCaptchaLibrary/badges/quality-score.png?b=master)](https://scrutinizer-ci.com/g/DarioSwain/ReCaptchaLibrary/?branch=master)
[![Code Coverage](https://scrutinizer-ci.com/g/DarioSwain/ReCaptchaLibrary/badges/coverage.png?b=master)](https://scrutinizer-ci.com/g/DarioSwain/ReCaptchaLibrary/?branch=master)
[![License](https://poser.pugx.org/dario_swain/re-captcha-library/license.svg)](https://packagist.org/packages/dario_swain/re-captcha-library)

You can find full documentation about Google reCAPTCHA API v2 [here](http://developers.google.com/recaptcha/intro).

##Installation

You can install this package with [Composer](http://getcomposer.org/).
Add next lines to your composer.json file:

``` json
{
    "require": {
        "dario_swain/re-captcha-library": "2.0.*"
    }
}
```

or you can use ```composer require```:

``` bash
composer require dario_swain/re-captcha-library 2.0.*
```


##Usage Example

####Displaying the widget:

```html
<html>
    <head>
        <script src='https://www.google.com/recaptcha/api.js'></script>
    </head>
    <body>
        <form method="post">
            <div class="g-recaptcha" data-sitekey="{RECAPTCHA SITE KEY}"></div>
            <br>
            <input type="submit" name="submit" value="Submit">
        </form>
    </body>
</html>
```

More about client integration you can find in [official docs](https://developers.google.com/recaptcha/docs/display).

####Verifying the user's response:

``` php
<?php

    $privateKey = 'RECAPTCHA PRIVATE KEY'; //You Google API private key
    $clientIp = $_SERVER['REMOTE_ADDR']; //Client IP Address
    $gReCaptchaResponse = $_POST['g-recaptcha-response']; //Google reCAPTCHA response

    $reCaptchaClient = new Client($privateKey);
    
    try {
        $success = $reCaptchaClient->validate($gReCaptchaResponse, $clientIp);
        
        if ($success) {
            //Submit form
        }
        
    } catch(ValidationException $e) {
        $validationError = $e->getMessage();
    }    

```

**Simple work example you can find in ```examples/index.php```.**


Custom Driver
-------------

You can change reCaptcha standard client to custom client implementation. In this case you can use 
```DS\Library\ReCaptcha\Http\Client\ClientInterface```. Also you can use any PSR7 comparability HTTP client like 

``` php
<?php
    class ProxyClient implements ClientInterface
    {
        {@inheritdoc}
        public function send(RequestInterface $request);
        {
            //Your business logic
        }
    }
    
    ...
    
    $proxyHttpClient = new ProxyClient();
    $reCaptchaClient = new Client($privateKey, $proxyHttpClient);
    
	$reCaptchaClient->validate($gReCaptchaResponse, $clientIp);

```

#Copyright

Copyright (c) 2015 Ilya Pokamestov <dario_swain@yahoo.com>.
