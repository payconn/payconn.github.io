# Payconn: Vakıf

[![Build Status](https://travis-ci.com/payconn/vakif.svg?branch=master)](https://travis-ci.com/payconn/vakif)

Vakıf (Vpos724) gateway for Payconn payment processing library

## Supported methods

- purchase
- authorize
- complete
- cancel
- refund

## Installation

    $ composer require payconn/vakif

## Purchase

Allows you to receive payments directly.

```php
use Payconn\Common\CreditCard;
use Payconn\Vakif\Token;
use Payconn\Vakif\Model\Purchase;
use Payconn\Vakif\Currency;
use Payconn\Vakif;

$token = new Token('YOUR_MERCHANT_ID', 'YOUR_TERMINAL_ID', 'YOUR_PASS');
$purchase = new Purchase();
$purchase->setTestMode(true);
$purchase->setCurrency(Currency::TRY);
$purchase->setAmount(100);
$purchase->setInstallment(1);
$purchase->setCreditCard(new CreditCard('4289450189088488', '2023', '04', '060'));
$response = (new Vakif($token))->purchase($purchase);
if($response->isSuccessful()){
    // success!
}
```

## Authorize: 3D Step-1

Starts payment flow by redirecting to your payment providers security page.

```php
use Payconn\Vakif\Token;
use Payconn\Vakif\Model\Authorize;
use Payconn\Vakif\Currency;
use Payconn\Common\CreditCard;

$token = new Token('YOUR_MERCHANT_ID', 'YOUR_TERMINAL_ID', 'YOUR_PASS');
$authorize = new Authorize();
$authorize->setTestMode(true);
$authorize->setCurrency(Currency::TRY);
$authorize->setAmount(1.26);
$authorize->setInstallment(3);
$authorize->setCreditCard(new CreditCard('4289450189088488', '2023', '04', '060'));
$authorize->setSuccessfulUrl('http://127.0.0.1:8000/successful');
$authorize->setFailureUrl('http://127.0.0.1:8000/failure');
$authorize->setCardBrand('100');
$authorize->generateOrderId(); // auto generated or $authorize->setOrderId('YOUR_ORDER_ID');
$response = (new \Payconn\Vakif($token))->authorize($authorize);
if($response->isSuccessful() && $response->isRedirection()){
    echo $response->getRedirectForm();
}
```

## Complete: 3D Step-2

It terminates the 3D security flow and makes the payment.

`returnParams` are the parameters returned from the bank.

```php
use Payconn\Vakif\Token;
use Payconn\Vakif\Model\Complete;
use Payconn\Vakif;

$token = new Token('YOUR_MERCHANT_ID', 'YOUR_TERMINAL_ID', 'YOUR_PASS');
$complete = new Complete();
$complete->setTestMode(true);
$complete->setReturnParams([
    'MerchantId' => '000100000013506', // $_POST['MerchantId']
    'Pan' => '4289450189088488', // $_POST['Pan']
    'Expiry' => '2304', // $_POST['Expiry']
    'PurchAmount' => '126', // $_POST['PurchAmount']
    'PurchCurrency' => '949', // $_POST['PurchCurrency']
    'VerifyEnrollmentRequestId' => 'ORDER1560106112', // $_POST['VerifyEnrollmentRequestId']
    'Xid' => 'vz6266srj9kdke7pcf84', // $_POST['Xid']
    'SessionInfo' => '1.26', // $_POST['SessionInfo']
    'Status' => 'Y', // $_POST['Status']
    'Cavv' => 'AAABAHdHEQAAAAAgYEcRAAAAAAA=', // $_POST['Cavv']
    'Eci' => '05', // $_POST['Eci']
    'InstallmentCount' => '3', // $_POST['InstallmentCount']
    'ErrorCode' => '', // $_POST['ErrorCode']
    'ErrorMessage' => '', // $_POST['ErrorMessage']
]);
$response = (new Vakif($token))->complete($complete);
if($response->isSuccessful()){
    // success!
}
```

## Refund

Performs refund for an old payment.

```php

if($response->isSuccessful()){
    // success!
}
```

## Cancel

Cancels a payment.

```php
if($response->isSuccessful()){
    // success!
}
```
