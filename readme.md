
<p align="center"><img src="https://laravel.com/assets/img/components/logo-laravel.svg"></p>

<br>

**Projeto Laravel JWT**

Esse projeto utiliza Laravel 5.7 com Mysql.

**Contents**

- [Instalação JWT](#Instalação-JWT)
- [Atualizar Token JWT](#Atualizar-Token-JWT)
- [Recuperar usuário](#Recuperar-usuário)

## Instalação JWT

Documentação disponível em [jwt-auth](https://github.com/tymondesigns/jwt-auth)

Seguir procedimentos em [jwt-auth](https://jwt-auth.readthedocs.io/en/develop/)

1. Instalação JWT via composer
```bash
composer require tymon/jwt-auth:dev-develop --prefer-source
```
2. Registrar `providers` em  `config/app.php`
```php
// config\app.php

'providers' => [

    ...

    Tymon\JWTAuth\Providers\LaravelServiceProvider::class,
]
```

2.1 Registrar alias
```php
// config\app.php

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

5.2 Cria autenticação jwt (FALTA!!!)


## Recuperar usuário

6. Recuperar usuário, adicione no método `authenticate()` do `AuthApiController`, em seguida passe a variave no array do json.
```php
// Recuperar usuario
$user = auth()->user();

// all good so return the token
return response()->json(compact('token', 'user'));
```

7. Usuário logado apartir do `Token`. Adicione método conforme manual.
```php
public function getAuthenticatedUser()
{
    try {

        if (! $user = JWTAuth::parseToken()->authenticate()) {
            return response()->json(['user_not_found'], 404);
        }

    } catch (Tymon\JWTAuth\Exceptions\TokenExpiredException $e) {

        return response()->json(['token_expired'], $e->getStatusCode());

    } catch (Tymon\JWTAuth\Exceptions\TokenInvalidException $e) {

        return response()->json(['token_invalid'], $e->getStatusCode());

    } catch (Tymon\JWTAuth\Exceptions\JWTException $e) {

        return response()->json(['token_absent'], $e->getStatusCode());

    }

    // the token is valid and we have found the user via the sub claim
    return response()->json(compact('user'));
}
```

7.1 Adicione rota para o método `getAuthenticatedUser()`. Deixar antes do grupo de rotas
```php
Route::get('me', 'Auth\AuthApiController@getAuthenticatedUser');
```
7.2 Faça teste pelo `Postmam`, passe a url `http://127.0.0.1:8000/api/me` com verto http `GET`, na guia `Headers` passe como parametro KEY: `Authorization` e VALUE: `Bearer + Token_Usuario` (token poderá ser obtido através da requisição 'POST' de login). Será retornado json com dados do usuário.

Exemplo retorno json:
```json
{
    "user": {
        "id": 1,
        "name": "José Santana",
        "email": "josesantana@gmail.com",
        "email_verified_at": null,
        "created_at": "2020-01-08 23:42:05",
        "updated_at": "2020-01-08 23:42:05"
    }
}
```
## Atualizar Token JWT
