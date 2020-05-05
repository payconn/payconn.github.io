# Payconn: QNB Finansbank

[![Build Status](https://travis-ci.com/ahmett/payconn-qnbfinansbank.svg?branch=master)](https://travis-ci.com/ahmett/payconn-qnbfinansbank)

QNB Finansbank gateway for Payconn payment processing library

## Installation

    $ composer require ahmett/payconn-qnbfinansbank


## Methods

The following methods are available:

Method | Description
--- | --- 
[purchase](#purchase)| Immediately capture an amount on the customer's card
[authorize](#authorize)| Authorize an amount on the customer's card
[complete](#complete)| Handle return from off-site gateways after purchase
[refund](#refund)| Refund an already processed transaction
[cancel](#cancel)| Void an already processed transaction

## Purchase

Immediately capture an amount on the customer's card.

```php
use Payconn\Common\CreditCard;
use Payconn\QNBFinansbank\Token;
use Payconn\QNBFinansbank\Currency;
use Payconn\QNBFinansbank\Model\Purchase;
use Payconn\QNBFinansbank;

$token = new Token('MERCHANT_ID', 'MERCHANT_PASS', 'USER_CODE', 'USER_PASS');
$creditCard = new CreditCard('NUMBER', 'EXPIRE_YEAR', 'EXPIRE_MONTH', 'CVV');
$purchase = new Purchase();

$purchase->setTestMode(true);
$purchase->setCurrency(Currency::TRY);
$purchase->setAmount(1);
$purchase->setInstallment(0);
$purchase->setCreditCard($creditCard);
$purchase->generateOrderId();

$response = (new QNBFinansbank($token))->purchase($purchase);

if ( $response->isSuccessful() ) {
    // success!
}
```

## Authorize

Authorize an amount on the customer's card.

```php
use Payconn\Common\CreditCard;
use Payconn\QNBFinansbank\Token;
use Payconn\QNBFinansbank\Currency;
use Payconn\QNBFinansbank\Model\Authorize;
use Payconn\QNBFinansbank;

$token = new Token('MERCHANT_ID', 'MERCHANT_PASS', 'USER_CODE', 'USER_PASS');
$creditCard = new CreditCard('NUMBER', 'EXPIRE_YEAR', 'EXPIRE_MONTH', 'CVV');
$authorize = new Authorize();

$authorize->setTestMode(true);
$authorize->setCurrency(Currency::TRY);
$authorize->setAmount(1);
$authorize->setInstallment(0);
$authorize->setCreditCard($creditCard);
$authorize->setSuccessfulUrl('http://127.0.0.1:8000/complete.php');
$authorize->setFailureUrl('http://127.0.0.1:8000/failure.php');
$authorize->generateOrderId();

$response = (new QNBFinansbank($token))->authorize($authorize);

if( $response->isSuccessful() && $response->isRedirection() ){
    echo $response->getRedirectForm();
}
```

## Complete

Handle return from off-site gateways after purchase.

```php
use Payconn\QNBFinansbank\Token;
use Payconn\QNBFinansbank\Model\Complete;
use Payconn\QNBFinansbank;

$token = new Token('MERCHANT_ID', 'MERCHANT_PASS', 'USER_CODE', 'USER_PASS');
$complete = new Complete();

$complete->setTestMode(true);
$complete->setReturnParams([
    'RequestGuid' => $_POST['RequestGuid'],
    'OrderId' => $_POST['OrderId'],
]);

$response = (new QNBFinansbank($token))->complete($complete);

if( $response->isSuccessful() ){
    // success!
}
```

## Refund

Refund an already processed transaction.

```php
use Payconn\QNBFinansbank\Token;
use Payconn\QNBFinansbank\Currency;
use Payconn\QNBFinansbank\Model\Refund;
use Payconn\QNBFinansbank;

$token = new Token('MERCHANT_ID', 'MERCHANT_PASS', 'USER_CODE', 'USER_PASS');
$refund = new Refund();

$refund->setTestMode(true);
$refund->setOrderId($_GET['order_id']);
$refund->setAmount($_GET['amount']);
$refund->setCurrency(Currency::TRY);

$response = (new QNBFinansbank($token))->refund($refund);

if( $response->isSuccessful() ){
    // success!
}
```

## Cancel

Void an already processed transaction.

```php
use Payconn\QNBFinansbank\Token;
use Payconn\QNBFinansbank\Currency;
use Payconn\QNBFinansbank\Model\Cancel;
use Payconn\QNBFinansbank;

$token = new Token('MERCHANT_ID', 'MERCHANT_PASS', 'USER_CODE', 'USER_PASS');
$cancel = new Cancel();

$cancel->setTestMode(true);
$cancel->setOrderId($_GET['order_id']);
$cancel->setCurrency(Currency::TRY);

$response = (new QNBFinansbank($token))->cancel($cancel);

if( $response->isSuccessful() ){
    // success!
}
```
