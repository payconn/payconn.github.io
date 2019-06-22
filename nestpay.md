# Payconn: Nestpay

[![Build Status](https://travis-ci.com/payconn/nestpay.svg?branch=master)](https://travis-ci.com/payconn/nestpay)

Nestpay (A Bank, Ak Bank, Anadolu Bank, Finans Bank, Halk Bank, ING Bank, İş Bank, Şeker Bank, Türk Ekonomi Bank (TEB), Ziraat Bank) gateway for Payconn payment processing library

## Supported methods

Method | Description
--- | --- 
[purchase](#purchase)| Immediately capture an amount on the customer's card
[authorize](#authorize)| Authorize an amount on the customer's card
[complete](#complete)| Handle return from off-site gateways after purchase
[refund](#refund)| Refund an already processed transaction
[cancel](#cancel)| Void an already processed transaction

## Installation

    $ composer require payconn/nestpay

## Purchase

Immediately capture an amount on the customer's card.

```php
use Payconn\Nestpay\Token;
use Payconn\Common\CreditCard;
use Payconn\Nestpay\Model\Purchase;
use Payconn\Nestpay\Currency;
use Payconn\AkBank;

$token = new Token('YOUR_CLIENT_ID', 'YOUR_USERNAME', 'YOUR_PASS');
$creditCard = new CreditCard('4355084355084358', '26', '12', '000');
$purchase = new Purchase();
$purchase->setTestMode(true);
$purchase->setCreditCard($creditCard);
$purchase->setAmount(1);
$purchase->setInstallment(1);
$purchase->setCurrency(Currency::TRY);
$response = (new AkBank($token))->purchase($purchase);
if($response->isSuccessful()){
    // success!
}
```

## Authorize

Authorize an amount on the customer's card.

```php
use Payconn\Nestpay\Token;
use Payconn\Common\CreditCard;
use Payconn\Nestpay\Model\Authorize;
use Payconn\Nestpay\Currency;
use Payconn\AkBank;

$token = new Token('YOUR_CLIENT_ID', 'YOUR_USERNAME', 'YOUR_PASS', 'YOUR_STORE_KEY');
$creditCard = new CreditCard('4355084355084358', '26', '12', '000');
$authorize = new Authorize();
$authorize->setFailureUrl('http://127.0.0.1:8000/failure');
$authorize->setSuccessfulUrl('http://127.0.0.1:8000/successful');
$authorize->setCreditCard($creditCard);
$authorize->setCurrency(Currency::TRY);
$authorize->setAmount(1);
$authorize->setInstallment(1);
$authorize->setTestMode(true);
$response = (new AkBank($token))->authorize($authorize);
if($response->isSuccessful() && $response->isRedirection()){
    echo $response->getRedirectForm();
}
```

## Complete

Handle return from off-site gateways after purchase.

```php
use Payconn\Nestpay\Token;
use Payconn\Nestpay\Model\Complete;
use Payconn\Nestpay\Currency;
use Payconn\AkBank;

$token = new Token('YOUR_CLIENT_ID', 'YOUR_USERNAME', 'YOUR_PASS', 'YOUR_STORE_KEY');
$complete = new Complete();
$complete->setTestMode(true);
$complete->setReturnParams([
    'xid' => 'ifmTW9moVmSL1v4v7CtufhWCcAY=', // $_POST['xid']
    'eci' => '05', // $_POST['eci']
    'cavv' => 'AAABBCYHAgAAAAARMAcCAAAAAAA=', // $_POST['cavv']
    'md' => '435508:7D4CC6608E4E5BCFD4DCE2C6A4ED', // $_POST['md']
    'oid' => '', // $_POST['oid']
]);
$complete->setCurrency(Currency::TRY);
$complete->setInstallment(1);
$complete->setAmount(1);
$response = (new AkBank($token))->complete($complete);
if($response->isSuccessful()){
    // success!
}
```

## Refund

Refund an already processed transaction.

```php
use Payconn\Nestpay\Token;
use Payconn\Nestpay\Model\Refund;
use Payconn\AkBank;

$token = new Token('YOUR_CLIENT_ID', 'YOUR_USERNAME', 'YOUR_PASS');
$refund = new Refund();
$refund->setTestMode(true);
$refund->setOrderId('YOUR_ORDER_ID');
$refund->setAmount('1'); // refund amount
$response = (new AkBank($token))->refund($refund);
if($response->isSuccessful()){
    // success!
}
```

## Cancel

Void an already processed transaction.

```php
use Payconn\Nestpay\Token;
use Payconn\Nestpay\Model\Cancel;
use Payconn\AkBank;

$token = new Token('YOUR_CLIENT_ID', 'YOUR_USERNAME', 'YOUR_PASS');
$cancel = new Cancel();
$cancel->setOrderId('YOUR_ORDER_ID');
$cancel->setTestMode(true);
$response = (new AkBank($token))->cancel($cancel);
if($response->isSuccessful()){
    // success!
}
```
