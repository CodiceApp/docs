# Installation

### Requirements
- PHP 5.5.9
- OpenSSL PHP Extension
- PDO PHP Extension
- Mbstring PHP Extension
- Tokenizer PHP Extension
- MySQL with InnoDB support

### Pre-packaged version
It is recommended method, which doesn't require any advanced skills. Whole configuration
is done using a web installer shipped with Codice.

1. Download [pre-packaged release](https://github.com/Sobak/Codice/releases)
2. Unpack files into desired location
3. Visit `yourdomain.com/codice/install` and follow the instructions

### From source
Use this method if you want to contribute or just feel more comfortable with using
console and having everything under control. 

Please note that this way of installing Codice requires you to have git, composer
and node.js with npm installed.

1. Clone repository (or just download sources if you don't need Git files)
2. `composer install` for installing PHP dependencies
3. `npm install` for installing node.js dependencies
4. `gulp assets` for building all frontend assets at once
5. `cp .env.example .env` and fill it with all required informations, of course
6. `php artisan db:install --username="John Doe" --email=john.doe@example.com --password=secret` will set
   a database for you (you can also just call `php artisan db:install` and go through interactive CLI wizard)

For development, you may be also interested in running `php artisan ide-helper:generate`
to make your IDE slightly less confused about all that Laravel magic.

### Other
You can mix above methods, clone the repo, install dependencies from the console
and then run online installer, if you wish.
