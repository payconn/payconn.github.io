# Payconn: iPara

[![Build Status](https://travis-ci.com/payconn/ipara.svg?branch=master)](https://travis-ci.com/payconn/ipara)

iPara gateway for Payconn payment processing library (Bonus, World, Axess, Maximum, Paraf, CardFinans, Sağlam Kart, Advantage)

## Installation

    $ composer require payconn/ipara

## Methods

The following methods are available:

Method | Description
--- | --- 
[purchase](#purchase)| Immediately capture an amount on the customer's card
[authorize](#authorize)| Authorize an amount on the customer's card
[complete](#complete)| Handle return from off-site gateways after purchase
[refund](#refund)| Refund an already processed transaction

## Purchase

Immediately capture an amount on the customer's card.

```php
use Payconn\Common\CreditCard;
use Payconn\Ipara\Token;
use Payconn\Ipara\Model\Purchase;
use Payconn\Ipara\Product;
use Payconn\Ipara;

$token = new Token('YOUR_PUBLIC_KEY', 'YOUR_PRIVATE_KEY');
$purchase = new Purchase();
$purchase->setTestMode(true);
$purchase->setAmount(100);
$purchase->setInstallment(1);
$purchase->setFirstName('Murat');
$purchase->setLastName('Sac');
$purchase->setEmail('muratsac@mail.com');
$purchase->addProduct((new Product('001', 'Test', 100)));
$purchase->setCreditCard((new CreditCard('4282209027132016', '2024', '12', '358'))
    ->setHolderName('Murat Sac'));
$purchase->generateOrderId(); // auto generated or `$purchase->setOrderId('YOUR_ORDER_ID')`
$response = (new Ipara($token))->purchase($purchase);
if($response->isSuccessful()){
    // success!
}
```

## Authorize

Authorize an amount on the customer's card.

```php
use Payconn\Common\CreditCard;
use Payconn\Ipara\Token;
use Payconn\Ipara\Model\Authorize;
use Payconn\Ipara\Product;
use Payconn\Ipara;

$token = new Token('YOUR_PUBLIC_KEY', 'YOUR_PRIVATE_KEY');
$authorize = new Authorize();
$authorize->setTestMode(true);
$authorize->setAmount(100);
$authorize->setInstallment(1);
$authorize->setFirstName('Murat');
$authorize->setLastName('Sac');
$authorize->setEmail('muratsac@mail.com');
$authorize->setSuccessfulUrl('http://127.0.0.1:8000/successful');
$authorize->setFailureUrl('http://127.0.0.1:8000/failure');
$authorize->addProduct((new Product('001', 'Test', 100)));
$authorize->setCreditCard((new CreditCard('4282209027132016', '2024', '12', '358'))
    ->setHolderName('MuratSac'));
$authorize->generateOrderId(); // auto generated or `$authorize->setOrderId('YOUR_ORDER_ID')`
$response = (new Ipara($token))->authorize($authorize);
if($response->isSuccessful() && $response->isRedirection()){
    echo $response->getRedirectForm();
}
```

## Complete

Handle return from off-site gateways after purchase.

```php
use Payconn\Ipara\Token;
use Payconn\Ipara\Model\Complete;
use Payconn\Ipara\Product;
use Payconn\Ipara;

$token = new Token('YOUR_PUBLIC_KEY', 'YOUR_PRIVATE_KEY');
$complete = new Complete();
$complete->setTestMode(true);
$complete->setFirstName('Murat');
$complete->setLastName('Sac');
$complete->setEmail('muratsac@mail.com');
$complete->addProduct((new Product('001', 'Test', 100)));
$complete->setReturnParams([
    'mode' => 'T', // $_POST['mode']
    'amount' => '10000', // $_POST['amount']
    'orderId' => 'ORD-1560882728', // $_POST['orderId']
    'threeDSecureCode' => '002Ddg+sMQOrdYxQdtsg', // $_POST['threeDSecureCode']
    'transactionDate' => '2019-06-18 21:28:50', // $_POST['transactionDate']
]);
$response = (new Ipara($token))->complete($complete);
if($response->isSuccessful()){
    // success!
}
```

## Refund

Refund an already processed transaction.

```php
use Payconn\Ipara\Token;
use Payconn\Ipara\Model\Refund;
use Payconn\Ipara;

$token = new Token('YOUR_PUBLIC_KEY', 'YOUR_PRIVATE_KEY');
$refund = new Refund();
$refund->setTestMode(true);
$refund->setAmount(100);
$refund->setOrderId('YOUR_ORDER_ID');
$refund->setTransactionDate('YOUR_TRANSACTION_DATE');
$refund->setOrderHash('YOUR_ORDER_HASH');
$response = (new Ipara($token))->refund($refund);
if($response->isSuccessful()){
    // success!
}
```
