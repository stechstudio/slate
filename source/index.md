---
title: ReproConnect API Reference

language_tabs:
  - shell
  - php

toc_footers:
  - <a href='mailto:support@reproconnect.com'>Contact Support</a>

includes:
  - companies
  - users
  - errors

search: true
---

# Introduction

```shell
Our shell code examples use curl
```

```php
PHP code examples use Guzzle. See http://guzzlephp.org
```

Welcome to the ReproConnect API. You can use our API to interact with (some of) your planroom.

This API is organized around <a href="https://en.wikipedia.org/wiki/Representational_state_transfer" target="_blank">REST</a> principles and the <a href="https://en.wikipedia.org/wiki/JSON" target="_blank">JSON</a> data format. Our API is designed to have predictable, resource-oriented URLs and to use HTTP response codes to indicate API errors.

We have code examples in Shell and PHP. You can view code examples in the dark area to the right, and you can switch the programming language of the examples with the tabs at the top.

# Base URL

The base URL for app API calls is: `https://api.qiplans.com/v1`

# Authentication

> To authenticate, use this code:

```shell
# With shell, you can just pass the correct header with each request
curl "https://api.qiplans.com/v1" \
  -H "x-api-key: YourAPIKey"
```

```php
<?php
$client = new GuzzleHttp\Client([
  'base_uri' => 'https://api.qiplans.com/v1',
  'headers' => ['x-api-key' => 'YourAPIKey']
]);
```

We use API keys to allow access to the API. If you don't have an API key already, please [contact support](mailto:support@reproconnect.com?subject=API+key) to request one.

The API key must be provided in the `x-api-key` header like this

`x-api-key: YourAPIKey`

<aside class="notice">
You must replace <code>YourAPIKey</code> with your personal API key.
</aside>

# Rate Limiting

By default all API keys are limited to 60 requests within a 5 minute period. If you hit your limit further requests will receive a `403 Forbidden` status.

Higher limits are available, fees may apply. Contact support with your requirements for details and pricing.
