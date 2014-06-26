ProxmoxVE API Client
========================

This **PHP 5.3+** library allows you to interact with your Proxmox server via API.


Installation
------------

Recomended installation is using [Composer], if you do not have [Composer] what are you waiting?

In the root of your project execute the following:

```sh
$ composer require zzantares/proxmoxve ~1.0
```

Or add this to your `composer.json` file:

```json
{
    "require": {
        "zzantares/proxmoxve": "~1.0"
    }
}
```

Then perform the installation:
```sh
$ composer install
```

Usage
-----

```php
<?php

// Require the autoloader
require_once 'vendor/autoload.php';

// Use the library namespaces
use ZzAntares\ProxmoxVE\Credentials;
use ZzAntares\ProxmoxVE\ProxmoxVE;

$server = 'your.server.tld';
$user = 'root';
$pass = 'secret';

// Create your Credentials object
$credentials = new Credentials($server, $user, $pass);

// realm and port defaults to 'pam' and '8006' but you can specify them like so
$credentials = new Credentials($server, $user, $pass, 'pve', '9009');

// Then simply pass your Credentials object when creating the API client object.
$proxmox = new ProxmoxVE($credentials);

$allNodes = $proxmox->get('/nodes');

print_r($allNodes);
```


Sample output:

```php
Array
(
    [data] => Array
        (
            [0] => Array
                (
                    [disk] => 2539465464
                    [cpu] => 0.031314446882002
                    [maxdisk] => 30805066770
                    [maxmem] => 175168446464
                    [node] => mynode1
                    [maxcpu] => 24
                    [level] => 
                    [uptime] => 139376
                    [id] => node/mynode1
                    [type] => node
                    [mem] => 20601992182
                )

        )

)
```

For the lazy ones it's possible to create a ProxmoxVE instance passing an associative array but you need to specify all fields including realm and port:

```php
<?php
// Once again require the autoloader
require_once 'vendor/autoload.php';

// You can define your credentials using an array
$credentials = array(
    'hostname' => 'your.server.tld',
    'username' => 'root',
    'password' => 'secret',
    'realm' => 'pam',
    'port' => '8006',
);

// Create ProxmoxVE instance by passing the $credentials array
$proxmox = new ZzAntares\ProxmoxVE\ProxmoxVE($credentials);

// Then you can use it, for example create a new user.

// Define params
$params = array(
    'userid' => 'new_user@pve',
    'comment' => 'Creating a new user',
    'password' => 'canyoukeepasecret?',
);

// Send request passing params
$result = $proxmox->post('/access/users', $params);

// According to the Proxmox API Docs, successful user creation returns null
if ($result) {
    error_log('Unable to create new proxmox user.');
}
```


Docs
----

On your proxmox client object you can use `get()`, `post()`, `put()` and `delete()` functions for all resources specified at [PVE2 API Documentation], params are passed as second parameter in an associative array.

*How does the Proxmox API works?* Consult the [ProxmoxVE API] article at the [Proxmox wiki].


License
-------

This project is released under the MIT License. See the bundled [LICENSE] file for details.


Donations
---------

If you feel guilty by using this open source software library for free, you can make a donation:

[![Bitcoin](https://lh6.googleusercontent.com/-otZw6Z5QKfQ/U6tqGsLBWYI/AAAAAAAAB-8/kvXncUJKKpU/w100-h36-no/bitcoin_accepted_here2.png)](https://blockchain.info/address/12g5CxCnWTc2dfo2EozCvfhi4QaFRj1vsX)

> 12g5CxCnWTc2dfo2EozCvfhi4QaFRj1vsX


[![Litecoin](https://lh3.googleusercontent.com/-s0Z7VSNScBU/U6tqGnAU5DI/AAAAAAAAB-0/fqaowxhOoBg/w100-h36-no/litecoin-accepted2.jpg)](http://ltc.blockr.io/address/info/LZuAcxLX4CBCPfLRtixatwRh234jNm3EyS)

> LZuAcxLX4CBCPfLRtixatwRh234jNm3EyS


[![PayPal](https://lh5.googleusercontent.com/-bQoXNrSpb8M/U6tqGqAFQUI/AAAAAAAAB-4/OkjaZ2ZQCUc/w100-h26-no/pp-logo-100px.png)](https://www.paypal.com/mx/cgi-bin/webscr?cmd=%5fsend%2dmoney&nav=1)

> zzantares@gmail.com


[![Fork me on GitHub](https://lh4.googleusercontent.com/-02ApStd24fs/U6t2PxxAQVI/AAAAAAAACAE/koI2tECOE60/w81-h32-no/GitHubForkButton.png)](https://github.com/ZzAntares/ProxmoxVE/fork)

> Fork and code! There is no better donation that your code contribution ;)


TODO
----
- Notify the user when a CNAME DNS record is provided as a hostname, Proxmox API seems to not work in this case.
- Save AuthToken data in serialized form to avoid ticket creation per request.
- Usage of the API need to allows you to specify return format.
	- json
	- extjs
	- html
	- text
- Use an abstracted OOP layer to access all Proxmox resources.
- Code useful tests ¬_¬!

[LICENSE]:https://github.com/ZzAntares/ProxmoxVE/blob/master/LICENSE
[PVE2 API Documentation]:http://pve.proxmox.com/pve2-api-doc/
[ProxmoxVE API]:http://pve.proxmox.com/wiki/Proxmox_VE_API
[Proxmox wiki]:http://pve.proxmox.com/wiki
[Composer]:https://getcomposer.org/
