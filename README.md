[![Build Status](https://travis-ci.org/crabstudio/bao-kim.svg?branch=master)](https://travis-ci.org/crabstudio/bao-kim) [![Latest Stable Version](https://poser.pugx.org/crabstudio/baokim/version)](https://packagist.org/packages/crabstudio/baokim) [![Latest Unstable Version](https://poser.pugx.org/crabstudio/baokim/v/unstable)](//packagist.org/packages/crabstudio/baokim) [![Total Downloads](https://poser.pugx.org/crabstudio/baokim/downloads)](https://packagist.org/packages/crabstudio/baokim)
# CakePHP 3.x: BaoKim payment gateway plugin

[Donate] Buy me a cup of coffee [![paypal](https://img.shields.io/badge/Donate-PayPal-green.svg)](https://www.paypal.com/cgi-bin/webscr?cmd=_donations&business=25FMH28VCF8V2&lc=VN&item_name=FlatAdmin%20plugin%20CakePHP%203&item_number=flat_admin&currency_code=USD&bn=PP%2dDonationsBF%3abtn_donateCC_LG%2egif%3aNonHosted)

## Installation

You can install this plugin into your CakePHP application using [composer](http://getcomposer.org).

The recommended way to install composer packages is:

```
composer require crabstudio/baokim
```
Or add the following lines to your application's **composer.json**:

```
"require": {
    "crabstudio/baokim": "^1.0"
}
```
followed by the command:

```
composer update
```

## Load plugin

Add this line to **Your_project\config\bootstrap.php**
```
Plugin::load('Crabstudio/BaoKim', ['bootstrap' => true]);
```
Or from command line:
```
bin/cake plugin load crabstudio/baokim
```
## Configure

Write to your configure:
```
$baokim = [
    'BaoKim' => [
        'merchant_id' => 'your_merchant_id',
        'secure_pass' => 'your_secret',
        'business' => 'your_business_email@example.com'
    ]
];
Cake\Core\Configure::write($baokim);
```

## Usage

Apply in your controller
```
public function checkout() {
	$this->loadComponent('Crabstudio/BaoKim.BaoKim');
	
	//your checkout logic here
	//$url_success route to checkoutSuccessfull function
	//$url_cancel route to checkoutCancel function

	$redirect_url = $this->BaoKim->createRequestUrl($order_id, $total_amount, $shipping_fee, $tax_fee, $order_description, $url_success, $url_cancel, $url_detail);
	return $this->redirect($redirect_url);
}

public function checkoutSuccessfull() {
	$this->request->allowMethod('get');
	$this->loadComponent('Crabstudio/BaoKim.BaoKim');
	$isValid = $this->BaoKim->verifyResponseUrl($this->request);
	if($isValid) {
		//Valid logic here
	}
	//Invalid logic here
}

public function checkoutCancel() {
	//Cancel logic here
}
```