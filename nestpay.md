# Nestpay

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

## 3D Secure Start - Authorize

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
