# Payconn: Nestpay

[![Build Status](https://travis-ci.com/payconn/nestpay.svg?branch=master)](https://travis-ci.com/payconn/nestpay)

Nestpay (A Bank, Ak Bank, Anadolu Bank, Finans Bank, Halk Bank, ING Bank, İş Bank, Şeker Bank, Türk Ekonomi Bank (TEB), Ziraat Bank) gateway for Payconn payment processing library

## Supported methods

- purchase
- authorize
- complete
- cancel
- refund

## Installation

    $ composer require payconn/nestpay

## Purchase

Allows you to receive payments directly.

```php
use Payconn\Common\CreditCard;
use Payconn\Garanti;
use Payconn\Garanti\Currency;
use Payconn\Garanti\Model\Purchase;
use Payconn\Garanti\Token;

$token = new Token('30691297', '7000679', '123qweASD/');
$creditCard = new CreditCard('4824894728063019', '23', '07', '172');
$purchase = new Purchase();
$purchase->setTestMode(true);
$purchase->setCreditCard($creditCard);
$purchase->setCurrency(Currency::TRY);
$purchase->setAmount(100);
$purchase->setInstallment(1);
$purchase->generateOrderId();
$response = (new Garanti($token))->purchase($purchase);
if($response->isSuccessful()){
    // success!
}
```

## Authorize

Starts payment flow by redirecting to your payment providers security page.

```php
use Payconn\Nestpay\Token;
use Payconn\Common\CreditCard;
use Payconn\Nestpay\Model\Authorize;
use Payconn\Nestpay\Currency;
use Payconn\AkBank;

$token = new Token('100100000', 'AKTESTAPI', 'AKBANK01', '123456');
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
if($response->isRedirection()){
    echo $response->getRedirectForm();
}
```

## Complete

It terminates the 3D security flow and makes the payment.

`returnParams` are the parameters returned from the bank.

```php
use Payconn\Nestpay\Token;
use Payconn\Nestpay\Model\Complete;
use Payconn\Nestpay\Currency;
use Payconn\AkBank;

$token = new Token('YOUR_CLIENT_ID', 'YOUR_USERNAME', 'YOUR_PASS', 'YOUR_STORE_KEY');
$complete = new Complete();
$complete->setTestMode(true);
$complete->setReturnParams([
    'xid' => 'ifmTW9moVmSL1v4v7CtufhWCcAY=',
    'eci' => '05',
    'cavv' => 'AAABBCYHAgAAAAARMAcCAAAAAAA=',
    'md' => '435508:7D4CC6608E4E5BCFD4DCE2C6A4ED113ED7E916D56E54302CD49012778C2652D6:4285:##100100000',
    'oid' => '',
]);
$complete->setCurrency(Currency::TRY);
$complete->setInstallment(1);
$complete->setAmount(1);
$response = (new AkBank($token))->complete($complete);
if($response->isSuccessful()){
    // success!
}
```
