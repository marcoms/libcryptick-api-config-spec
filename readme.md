# libcryptick JSON API configuration

This specification defines the required and optional objects for use by the
[libcryptick](https://github.com/marcoms/libcryptick) library.

Each object listed in this document is of the format 'name (type)'. Each object
is also followed by a line stating if said object is either required
('required') or optional ('optional').

----

## Definitions

### Format array

Objects of type 'format `Array`' are simple `Array`s of `String`s, which have their elements concatenated into a string, with lone `"{c}"` and `"{e}"` elements replaced by the desired coin and exchange, respectively.

For example:

`["https://api.mintpal.com/v2/market/stats/", "{c}", "/", "{e}"]` becomes `"https://api.mintpal.com/v2/market/stats/DOGE/BTC"`

Where coin = `"DOGE"` and exchange = `"BTC"`.

## Objects

### `name` (`String`)

*required*

The API's name.

#### Examples

* `"MintPal"`
* `"AllCoin"`
* `"BTC-e"`
* `"Cryptsy"`

### `url` (`String` / format `Array`)

*required*

Link to JSON provided by the API.

#### Examples

* `["https://btc-e.com/api/2/", "{c}", "_", "{e}", "/ticker"]`
* `["https://api.mintpal.com/v2/market/stats/", "{c}", "/", "{e}"]`
* `["https://www.allcoin.com/api2/pair/", "{c}", "_", "{e}"]`
* `"https://www.allcoin.com/api2/pairs"`

### `status_success` (`String` / `Number` (integer) / `Boolean`)

*required if `paths.status` is defined*

The value in `paths.status` denoting a successful API response.

#### Examples

* `1`
* `true`
* `"success"`
* `"good"`

### `number_format` (`String`)

*required*

A string specifying how number values are represented.

Possible values are:

* `"string"`
* `"real"`

### `paths` (`Object`)

*required*

Object providing locations in JSON for common API information, where each element in the array is the parent object of the subsequent element in the array.

#### `paths.status` (`Array` of `String`s / format `Array`s)

*optional*

Location to object giving the status of the API response, where a positive response is provided by `success`.

##### Examples

* `["status"]`
* `["result"]`

#### `paths.status_desc` (`Array` of `String`s / format `Array`s)

*optional*

The string providing a description for an erroneous API call.

##### Examples

* `["message"]`
* `["data"]`

#### `paths.buy` (`Array` of `String`s / format `Array`s)

*required*

Location to the top asking price.

##### Examples

* `["data", "top_ask"]`
* `["data", ["{c}", "_", "{e}"], "top_ask"]`

#### `paths.sell` (`Array` of `String`s / format `Array`s)

*required*

Location to the top bidding price.

##### Examples

* `["data", "top_bid"]`
* `["data", ["{c}", "_", "{e}"], "top_bid"]`

## Example API configuration

	{
		"name": "MintPal"
		, "url": ["https://api.mintpal.com/v2/market/stats/", "{c}", "/", "{e}"]
		, "status_success": "success"
		, "paths": {
			, "status": ["status"]
			, "status_description": ["message"]
			, "buy": ["data", "top_ask"]
			, "sell": ["data", "top_bid"]
		}
	}

----

Format `Array`s is generally used in `url` when the API used provided a single market API, but used in `paths`' children when a single market API is not availible, but only a whole market summary. In general, single market APIs are preferable, as they reduce bandwidth and are less complex.

Copyright (c) 2014 Marco Scannadinari <m@scannadinari.co.uk>
