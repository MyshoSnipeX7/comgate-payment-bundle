## Symfony Bundle

A symfony bundle for Comgate payments

Installation
------------

### Step 1: Download MyshoComgateBundle using composer

Require the `mysho/comgate-bundle` with composer [Composer](http://getcomposer.org/).

```bash
$ composer require mysho/comgate-bundle
```

### Step 2: Enable the bundle

Enable the bundle in the kernel:

```php
<?php

// app/AppKernel.php
public function registerBundles()
{
    $bundles = array(
        // ...
        new Mysho\ComgateBundle\MyshoComgateBundle(),
        // ...
    );
}
```

### Step 3: Configure the MyshoComgateBundle

Below is a minimal example of the configuration necessary to use the
`MyshoComgateBundle` in your application:

```yml
# .env

###> mysho/comgate-bundle ###
MERCHANT_ID="your merchant id"
SECRET_KEY="secret key from comgate dashboard"
TEST_MODE=false
###< mysho/comgate-bundle ###
```

### Step 4: Usage of MyshoComgateBundle

```php
# CartController

/**
     * @Route("/cart/payment", name="cart_payment")
     * @param ComgateConnector $comgate
     * @param Request
     * @return Response
     */
    public function payment(ComgateConnector $comgate): Response
    {
        
        $payment = new CreatePayment('PRICE', 'Your order ID', $this->getUser()->getEmail(), 'Some product');
        // to create payment on background according to API requirements
        $payment->setPrepareOnly(true);
        $response = $comgate->send($payment);
        if($response->getMessage()=="OK"){
            // do something with cart
            return $this->redirect($response->getRedirectUrl());
        }

        return $this->render('cart/payment.html.twig', []);
    }

```
