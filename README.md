# Invoice Ninja SDK

### Installation

Add the Invoice Ninja SDK

    composer require invoiceninja/sdk-php

### Setup
```php

    require __DIR__ . '/vendor/autoload.php';

    use InvoiceNinja\Config as NinjaConfig;
    use InvoiceNinja\Models\Client;

    NinjaConfig::setURL('https://ninja.dev/api/v1');
    NinjaConfig::setToken('Your Token');
    NinjaConfig::setPerPage(15);
    NinjaConfig::setPage(1);
```
- To connect to the hosted version use `https://app.invoiceninja.com/api/v1` as the URL.
- You can either use the [free hosted trial](https://app.invoiceninja.com/invoice_now?sign_up=true&redirect_to=/settings/api_tokens) or [install the app](https://www.invoiceninja.com/self-host/) to create an API token.

### Supports

- Clients
- Invoices/Quotes
- Products
- Payments
- Tasks
- Vendors
- Expenses
- TaxRates
- Credits

### Retrieving Models

Pagination  
To iterate through more than 1 page of results, increment `NinjaConfig::setPage(1)` value until an empty array of results is returned.  Adjust the number of results per page using the `NinjaConfig::setPerPage(15);` value *(maximum 5000)*.

Retrieve all clients
```php
    $clients = Client::all();
```

Retrieve a client by its primary key.
```php
    $client = Client::find(1);
```

Retrieving an invoice by it's number:
```php
    $invoice = Invoice::findByCustomQuery(["invoice_number" => "0123"]);
```

### Inserting & Updating Models

Create a new client
```php
    $client = new Client('test@example.com');
    $client->save();
```
Update an existing client
```php
    $client->vat_number = '123456';
    $client->save();
```
Create an invoice
```php
    $invoice = $client->createInvoice();
    $invoice->addInvoiceItem('Item', 'Some notes', 10);
    $invoice->save();
```
Download an invoice PDF
```php
    $pdf = $invoice->download();
```
### Deleting Models
```php
    $client->archive();
    $client->delete();
    $client->restore();
```
## Events

Register subscription for new client
```php
    Client::subscribeCreate('http://example.com/...');
```
Convert posted data to a model
```php
    $input = file_get_contents('php://input'); 
    $client = Client::hydrate($input);
```
*Currently supported for clients, invoices, quotes and payments*

## Statics

*Retrieve the static dataset Invoice Ninja uses*

Get all statics
```php
    Statics::all();
```
Get specific static
```php
    Statics::countries();
```
List of available statics
```php
    Statics::currencies();
    Statics::sizes();
    Statics::timezones();
    Statics::dateFormats();
    Statics::datetimeFormats();
    Statics::languages();
    Statics::paymentTerms();
    Statics::paymentTypes();
    Statics::countries();
    Statics::invoiceDesigns();
    Statics::invoiceStatus();
    Statics::frequencies();
    Statics::gateways();
    Statics::fonts();
    Statics::banks();
```
