# Bitrix CI Build

[![PHPUnit](https://github.com/sheerockoff/bitrix-ci/workflows/PHPUnit/badge.svg?branch=master)](https://github.com/sheerockoff/bitrix-ci/actions)
[![Code Size](https://img.shields.io/github/languages/code-size/sheerockoff/bitrix-ci.svg)](https://packagist.org/packages/sheerockoff/bitrix-ci)

Минимальный сборка [Bitrix](https://www.1c-bitrix.ru/products/cms/index.php) для использования в тестировании.

## Быстрый старт

Устанавливаем.

```bash
composer require --dev sheerockoff/bitrix-ci
```

Подключаем зависимости.

```php
<?php

require 'vendor/autoload.php';
```

Подключение к базе данных настраивается переменными окружения `MYSQL_HOST`, `MYSQL_DATABASE`, `MYSQL_USER` и `MYSQL_PASSWORD`.
Они могут быть переопределены в PHP.

```php
putenv('MYSQL_HOST=localhost');
putenv('MYSQL_DATABASE=bitrix_ci');
putenv('MYSQL_USER=user');
putenv('MYSQL_PASSWORD=password');
```

Разворачиваем дамп MySQL.

```php
\Sheerockoff\BitrixCi\Bootstrap::migrate();
```

Подключаем Bitrix.

```php
\Sheerockoff\BitrixCi\Bootstrap::bootstrap();
```

Тестируем код, который зависит от API Bitrix.

```php
/**
 * @param array $stack
 * @return array
 */
public function testCanGetBitrixElement(array $stack)
{
    $element = CIBlockElement::GetList(null, ['ID' => $stack['id']])->GetNextElement();
    $this->assertInstanceOf(_CIBElement::class, $element);
    
    $fields = $element->GetFields();
    $this->assertEquals($stack['id'], $fields['ID']);
    
    return $stack;
}
```

## Список подключенных модулей

* [Главный модуль](https://dev.1c-bitrix.ru/api_help/main/index.php)
* [Информационные блоки](https://dev.1c-bitrix.ru/api_help/iblock/index.php)
* [Highload-блоки](https://dev.1c-bitrix.ru/api_help/hlblock/index.php)
* [Интернет-магазин](https://dev.1c-bitrix.ru/api_help/sale/index.php)
* [Торговый каталог](https://dev.1c-bitrix.ru/api_help/catalog/index.php)
* [Валюты](https://dev.1c-bitrix.ru/api_help/currency/index.php)
