# Payconn: Wirecard

[![Build Status](https://travis-ci.com/payconn/wirecard.svg?branch=master)](https://travis-ci.com/payconn/wirecard)

Wirecard gateway for Payconn payment processing library

## Installation

    $ composer require payconn/wirecard

## Methods

The following methods are available:

Method | Description
--- | --- 
[purchase](#purchase)| Immediately capture an amount on the customer's card
[authorize](#authorize)| Authorize an amount on the customer's card
[refund](#refund)| Refund an already processed transaction

## Purchase

Immediately capture an amount on the customer's card.

```php
use Payconn\Common\CreditCard;
use Payconn\Wirecard;
use Payconn\Wirecard\Model\Purchase;
use Payconn\Wirecard\Token;

$token = new Token('<YOUR_USER_CODE>', '<YOUR_PIN>');
$purchase = new Purchase();
$purchase->setTestMode(true);
$purchase->setAmount(100);
$purchase->setInstallment(1);
$purchase->setDescription('Test payment');
$purchase->setCreditCard((new CreditCard('4111111111111111', '2024', '01', '123'))->setHolderName('Murat Sac'));
$purchase->generateOrderId();
$response = (new Wirecard($token))->purchase($purchase);
if($response->isSuccessful()){
    // success!
}
```

## Authorize

Authorize an amount on the customer's card.

```php
use Payconn\Common\CreditCard;
use Payconn\Wirecard;
use Payconn\Wirecard\Model\Authorize;
use Payconn\Wirecard\Token;

$token = new Token('<YOUR_USER_CODE>', '<YOUR_PIN>');
$authorize = new Authorize();
$authorize->setTestMode(true);
$authorize->setAmount(100);
$authorize->setInstallment(1);
$authorize->setDescription('Test payment');
$authorize->setCreditCard((new CreditCard('4111111111111111', '2024', '01', '123'))->setHolderName('Murat Sac'));
$authorize->setSuccessfulUrl('https://SUCCES-URL');
$authorize->setFailureUrl('https://FAILURE-URL');
$response = (new Wirecard($token))->authorize($authorize);
if($response->isSuccessful() && $response->isRedirection()){
    echo $response->getRedirectForm();
}
```

## Refund

Refund an already processed transaction.

```php
use Payconn\Wirecard;
use Payconn\Wirecard\Model\Refund;
use Payconn\Wirecard\Token;

$token = new Token('<YOUR_USER_CODE>', '<YOUR_PIN>');
$refund = new Refund();
$refund->setTestMode(true);
$refund->setOrderId('<YOUR-ORDER-ID>');
$response = (new Wirecard($token))->refund($refund);
if($response->isSuccessful()){
    // success!
}
```
