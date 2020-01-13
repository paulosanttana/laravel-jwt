
<p align="center"><img src="https://laravel.com/assets/img/components/logo-laravel.svg"></p>

<br>

**Projeto Laravel JWT**

Esse projeto utiliza Laravel 5.7 com Mysql.

**Contents**

- [Instalação JWT](#Instalação-JWT)


## Instalação JWT

Documentação disponível em [jwt-auth](https://github.com/tymondesigns/jwt-auth)

Seguir procedimentos em [jwt-auth](https://jwt-auth.readthedocs.io/en/develop/)

1. Instalação JWT via composer
```bash
composer require tymon/jwt-auth:dev-develop --prefer-source
```
2. Registrar `providers` em  `config/app.php`
```php
// config/app.php

'providers' => [

    ...

    Tymon\JWTAuth\Providers\LaravelServiceProvider::class,
]
```

2.1 Registrar alias
```php
// config/app.php

'aliases' => [

    ...

    'JWTAuth' => Tymon\JWTAuth\Facades\JWTAuth::class,
],    
```

3. Gerar arquivo de configuração jwt. Será criado aquivo em `config/jwt.php`.
```bash
php artisan vendor:publish --provider="Tymon\JWTAuth\Providers\LaravelServiceProvider"
```

4. Criar chave de segurança dentro do `.env`.
```bash
php artisan jwt:secret
```

5. Criar controller dentro da pasta `Auth`
```bash
php artisan make:controller Auth\\AuthApiController
```

5.1 Criar rota
```php
// routes\api.php

Route::post('auth', 'Auth\AuthApiController@authenticate');
```

5.2 Cria autenticação jwt