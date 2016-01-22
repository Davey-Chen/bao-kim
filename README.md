# CakePHP 3.x: BaoKim payment gateway plugin

## Introduction

BaoKim payment gateway plugin for CakePHP 3.x

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

## Configure

Write to your configure:
```
$baokim = [
    'BaoKim' => [
        'merchant_id' => 'your_merchant_id',
        'secure_pass' => 'your_secret',
    ]
];
Cake\Core\Configure::write($baokim);
```

## Usage

Apply in your controller
```
public function checkout() {
	$this->loadComponent('Crabstudio/BaoKim');
	
	//your checkout logic here
	//$url_success route to checkoutSuccessfull function
	//$url_cancel route to checkoutCancel function

	$redirect_url = $this->BaoKim->createRequestUrl($order_id, $business, $total_amount, $shipping_fee, $tax_fee, $order_description, $url_success, $url_cancel, $url_detail);
	return $this->redirect($redirect_url);
}

public function checkoutSuccessfull() {
	$this->request->allowMethod('get');
	$this->loadComponent('Crabstudio/BaoKim');
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