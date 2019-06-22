# Payconn: Garanti

[![Build Status](https://travis-ci.com/payconn/garanti.svg?branch=master)](https://travis-ci.com/payconn/garanti)

Garanti (GVP) gateway for Payconn payment processing library

## Installation

    $ composer require payconn/garanti

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
use Payconn\Garanti;
use Payconn\Garanti\Currency;
use Payconn\Garanti\Model\Purchase;
use Payconn\Garanti\Token;

$token = new Token('YOUR_TERMINAL_ID', 'YOUR_MERCHANT_ID', 'YOUR_PASS');
$creditCard = new CreditCard('4824894728063019', '23', '07', '172');
$purchase = new Purchase();
$purchase->setTestMode(true);
$purchase->setCreditCard($creditCard);
$purchase->setCurrency(Currency::TRY);
$purchase->setAmount(100);
$purchase->setInstallment(1);
$purchase->generateOrderId(); // auto generated or `$purchase->setOrderId('YOUR_ORDER_ID')`
$response = (new Garanti($token))->purchase($purchase);
if($response->isSuccessful()){
    // success!
}
```

## Authorize

Authorize an amount on the customer's card.

```php
use Payconn\Common\CreditCard;
use Payconn\Garanti;
use Payconn\Garanti\Currency;
use Payconn\Garanti\Model\Authorize;
use Payconn\Garanti\Token;

$token = new Token('YOUR_TERMINAL_ID', 'YOUR_MERCHANT_ID', 'YOUR_PASS', 'YOUR_STORE_KEY');
$creditCard = new CreditCard('4282209004348015', '22', '08', '123');
$authorize = new Authorize();
$authorize->setTestMode(true);
$authorize->setCreditCard($creditCard);
$authorize->setCurrency(Currency::TRY);
$authorize->setAmount(100);
$authorize->setInstallment(1);
$authorize->setSuccessfulUrl('https://webhook.site/d123fc90-9aa6-42b5-b83c-846ecf739edc');
$authorize->setFailureUrl('https://webhook.site/d123fc90-9aa6-42b5-b83c-846ecf739edc');
$authorize->generateOrderId(); // auto generated or `$authorize->setOrderId('YOUR_ORDER_ID')`
$response = (new Garanti($token))->authorize($authorize);
if($response->isSuccessful() && $response->isRedirection()){
    echo $response->getRedirectForm();
}
```

## Complete

Handle return from off-site gateways after purchase.

```php
use Payconn\Garanti;
use Payconn\Garanti\Model\Complete;
use Payconn\Garanti\Response\CompleteResponse;
use Payconn\Garanti\Token;

$token = new Token('YOUR_TERMINAL_ID', 'YOUR_MERCHANT_ID', 'YOUR_PASS');
$complete = new Complete();
$complete->setTestMode(true);
$complete->setReturnParams([
    'clientid' => '30691297', // $_POST['clientid']
    'txnamount' => '10000', // $_POST['txnamount']
    'terminalprovuserid' => 'PROVAUT', // $_POST['terminalprovuserid']
    'terminaluserid' => 'PROVAUT', // $_POST['terminaluserid']
    'customeripaddress' => '127.0.0.1', // $_POST['customeripaddress']
    'orderid' => 'GVP1559853532', // $_POST['orderid']
    'txntype' => 'sales', // $_POST['txntype']
    'txninstallmentcount' => '1', // $_POST['txninstallmentcount']
    'txncurrencycode' => '949', // $_POST['txncurrencycode']
    'cavv' => 'jCm0m+u/0hUfAREHBAMBcfN+pSo=', // $_POST['cavv']
    'eci' => '02', // $_POST['eci']
    'xid' => 'RszfrwEYe/8xb7rnrPuh6C9pZSQ=', // $_POST['xid']
    'md' => 'G1YfkxEZ8Noemg4MRspO20vEiXaEk51Af', // $_POST['md']
]);
/** @var CompleteResponse $response */
$response = (new Garanti($token))->complete($complete);
if($response->isSuccessful()){
    // success!
}
```

## Refund

Refund an already processed transaction.

```php
use Payconn\Garanti;
use Payconn\Garanti\Model\Refund;
use Payconn\Garanti\Token;

$token = new Token('YOUR_TERMINAL_ID', 'YOUR_MERCHANT_ID', 'YOUR_PASS');
$refund = new Refund();
$refund->setTestMode(true);
$refund->setAmount(50);
$refund->setOrderId('YOUR_ORDER_ID');
$refund->setReturnedOrderId('YOUR_RETURNED_ORDER_ID');
$response = (new Garanti($token))->refund($refund);
if($response->isSuccessful()){
    // success!
}
```

## Cancel

Void an already processed transaction.

```php
use Payconn\Garanti;
use Payconn\Garanti\Model\Cancel;
use Payconn\Garanti\Token;

$token = new Token('YOUR_TERMINAL_ID', 'YOUR_MERCHANT_ID', 'YOUR_PASS');
$cancel = new Cancel();
$cancel->setTestMode(true);
$cancel->setAmount(100);
$cancel->setReturnedOrderId('YOUR_RETURNED_ORDER_ID');
$cancel->generateOrderId();
$response = (new Garanti($token))->cancel($cancel);
if($response->isSuccessful()){
    // success!
}
```
