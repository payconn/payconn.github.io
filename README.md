[![Build Status](https://travis-ci.com/payconn/common.svg?branch=master)](https://travis-ci.com/payconn/common)

[Payconn](https://github.com/payconn/common) is a framework agnostic, multi-gateway payment
processing library for PHP. This package implements common classes required by Payconn.

## Payment Gateways

All payment gateways must implement [GatewayInterface](https://github.com/payconn/common/blob/master/src/Common/GatewayInterface.php), and will usually
extend [AbstractGateway](https://github.com/payconn/common/blob/master/src/Common/AbstractGateway.php) for basic functionality.

The following gateways are available:

Gateway | Composer Package | Author | Build Status 
--- | --- | --- | ---
[Nestpay (EST)](https://payconn.org/nestpay) | payconn/nestpay | [Murat SAÇ](https://github.com/muratsac) | [![Build Status](https://travis-ci.com/payconn/nestpay.svg?branch=master)](https://travis-ci.com/payconn/nestpay)
[Vakıf](https://payconn.org/vakif) | payconn/vakif | [Murat SAÇ](https://github.com/muratsac) | [![Build Status](https://travis-ci.com/payconn/vakif.svg?branch=master)](https://travis-ci.com/payconn/vakif)
[iPara](https://payconn.org/ipara) | payconn/ipara | [Murat SAÇ](https://github.com/muratsac) | [![Build Status](https://travis-ci.com/payconn/ipara.svg?branch=master)](https://travis-ci.com/payconn/ipara)
[Garanti](https://payconn.org/garanti) | payconn/garanti | [Murat SAÇ](https://github.com/muratsac) | [![Build Status](https://travis-ci.com/payconn/garanti.svg?branch=master)](https://travis-ci.com/payconn/garanti)

## Installation

    $ composer require payconn/payconn

## Basic Usage

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

## Support

If you are having general issues with Payconn, we suggest posting on
[Stack Overflow](http://stackoverflow.com/). Be sure to add the

If you believe you have found a bug, please report it using the [GitHub issue tracker](https://github.com/payconn/common/issues),
or better yet, fork the library and submit a pull request.


## Security

If you discover any security related issues, please email muratsac@mail.com instead of using the issue tracker.


## License

The MIT License (MIT). Please see [License File](LICENSE.md) for more information.
