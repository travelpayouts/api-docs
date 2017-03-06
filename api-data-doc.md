# Data Access API
Travelpayouts Data API – the way to get travel insights for your site or blog. You can get flight price trends and find popular destinations for your customers.

Dear partners! Attention, the data is transferred from the cache, so it is recommended to use them to generate static pages. 

To access the API you must pass your token in the X-Access-Token header or in the token parameter. To obtain a token for the Data Access API, go to http://www.travelpayouts.com/developers/api.

Dates are accepted in the formats YYYY-MM and YYYY-MM-DD.

The server response is always sent in json format with the following structure:

  - **success** – true for successful request, false in case of errors;
  - **data** – result of the request; in case of an error is equal to null;
  - **error** – short description of the error that prevented request completion; for successful request is equal to null.

Dates and times are given in UTC, formatted according to [ISO 8601](https://ru.wikipedia.org/wiki/ISO_8601). Prices are given in rubles as of when the ticket is put in the search results. It is not recommended to use expired prices (the approximate expiration date is given in the value of the expires_at parameter).

**Important**. We strongly urge receiving data in compressed GZIP format, which saves a significant amount of time in receiving the response. To get data in compressed form, send the header Accept-Encoding: gzip, deflate.

To obtain access to the API for searching for plane tickets and hotels, [send a request](https://support.travelpayouts.com/hc/en-us/requests/new).

## The prices for the airline tickets
Brings back to the list of prices, found by our users during the recent 48 hours according to the filters used.

### Request

http://api.travelpayouts.com/v2/prices/latest?currency=usd&period_type=year&page=1&limit=30&show_to_affiliates=true&sorting=price&trip_class=0&token=PutHereYourToken

#### Request parameters

  - **currency** — the airline tickets currency. The default value — RUB.
  - **origin** — the point of departure. The IATA city code or the country code. The length - from 2 to 3 symbols.
  - **destination** — the point of destination. The IATA city code or the country code. The length - from 2 to 3 symbols.

**Note!** If the point of departure and the point of destination are not specified, then the API shall bring 30 the most cheapest tickets, which have been found during the recent 48 hours, back.

 - **beginning_of_period** — the beginning of the period, within which the dates of departure fall (in the YYYY-MM-DD format, for example, 2016-05-01). Must be specified if **period_type** is equal to **month**.
 - **period_type** — the period, for which the tickets have been found (the required parameter):
 - **year** — for the whole time;
 - **month** — for a month.
 - **one_way** — true — one way, false — back-to-back. The default value — false.
 - **page** — a page number. The default value — 1.
 - **limit** — the total number of records on a page. The default value — 30. The maximum value — 1000.
 - **show_to_affiliates** — false — all the prices, true — just the prices, found using the partner marker (recommended). The default value — true.
 - **sorting** — the assorting of prices:
-**price** — by the price (the default value). For the directions **city — city** assorting by the price is possible only;
 - **route** — by the popularity of a route;
 - **distance_unit_price** — by the price for 1 km.
 - **trip_class** — the flight class:
  - **0** — The economy class (the default value);
  - **1** — The business class;
  - **2** — The first class.
 - **trip_duration** — the length of stay in weeks or days (for period_type=day).
 - **token** — individual affiliate token.

### Response example
{
	"success":true,
	"data":[{
		"show_to_affiliates":true,
		"trip_class":0,
		"origin":"WMI",
		"destination":"WRO",
		"depart_date":"2015-12-07",
		"return_date":"2015-12-13",
		"number_of_changes":0,
		"value":1183,
		"found_at":"2015-09-22T14:08:45+04:00",
		"distance":298,
		"actual":true
	}]
} 

#### Response parameters
 - **origin** — the point of departure.
 - **destination** — the point of destination.
 - **show_to_affiliates** — false - all the prices, true — just the prices, found using the partner marker (recommended). The default value — true.
 - **trip_class** — the flight class:
  - **0** — the economy class,
  - **1** — the business class,
  - **2** — the first class.
 - **depart_date** — the date of departure.
 - **return_date** — the date of return.
 - **number_of_changes** — the number of transfers.
 - **value** — the cost of a flight, in the currency specified.
 - **found_at** — the time and the date, for which a ticket was found.
 - **distance** — the distance between the point of departure and the point of destination.
 - **actual** — the actuality of an offer.
 - **token** — individual affiliate token.


## The calendar of prices for a month

Brings the prices for each day of a month, grouped together by the number of transfers, back.

### Request
http://api.travelpayouts.com/v2/prices/month-matrix?currency=usd&origin=LED&destination=HKT&show_to_affiliates=true&token=PutHereYourToken

#### Request parameters
 - **currency** — the airline tickets currency. The default value - RUB.
 - **origin** — the point of departure. The IATA city code or the country code. The length - from 2 to 3 symbols.
 - **destination** — the point of destination. The IATA city code or the country code. The length - from 2 to 3 symbols.

**Note!** If the point of departure and the point of destination are not specified, then the API shall bring 30 the most cheapest tickets, which have been found during the recent 48 hours, back.

 - **month** — the beginning of the month in the YYYY-MM-DD format.
 - **show_to_affiliates** — false - all the prices, true - just the prices, found using the partner marker (recommended). The default value - true.
 - **token** — individual affiliate token.

### Response example
{{
    "success":true,
    "data":[{
        "show_to_affiliates":true,
        "trip_class":0,
        "origin":"LED",
        "destination":"HKT",
        "depart_date":"2015-10-01",
        "return_date":"",
        "number_of_changes":1,
        "value":29127,
        "found_at":"2015-09-24T00:06:12+04:00",
        "distance":8015,
        "actual":true
    }]
}}

#### Response parameters
 - **origin** — the point of departure.
 - **destination** — the point of destination.
 - **show_to_affiliates** — false - all the prices, true - just the prices, found using the partner marker (recommended). The default value - true.
 - **trip_class** — the flight class:
  - **0** — The economy class,
  - **1** — The business class,
  - **2** — The first class.
 - **depart_date** — the date of departure.
 - **return_date** — the date of return.
 - **number_of_changes** — the number of transfers.
 - **value** — the cost of a flight, in the currency specified.
 - **found_at** — the time and the date, for which a ticket was found.
 - **distance** — the distance between the point of departure and the point of destination.
 - **actual** — the actuality of an offer.

## The prices for the alternative directions

Brings the prices for the directions between the nearest to the target cities back.

### Request
http://api.travelpayouts.com/v2/prices/nearest-places-matrix?currency=usd&origin=LED&destination=HKT&show_to_affiliates=true&token=PutHereYourToken

#### Request parameters
 - **currency** — the airline tickets currency. The default value - RUB.
 - **origin** — the point of departure. The IATA city code or the country code. The length - from 2 to 3 symbols.
 - **destination** — the point of destination. The IATA city code or the country code. The length - from 2 to 3 symbols.
 - **limit** — the number of variants entered, from 1 to 20. Where 1 – is just the variant with the specified points of departure and the points of destination.
 - **show_to_affiliates** — false - all the prices, true - just the prices, found using the partner marker (recommended). The default value - true.
 - ** depart_date** *(optional)* — Day or month of departure (yyyy-mm-dd or yyyy-mm).
 - **return_date** *(optional)* — Day or month of return (yyyy-mm-dd or yyyy-mm).
 - **flexibility** — expansion of the range of dates upward or downward. The value may vary from 0 to 7, where 0 shall show the variants for the dates specified, 7 – all the variants found for a week prior to the specified dates and a week after.
 - **distance** — the number of variants entered, from 1 to 20. Where 1 – is just the variant with the specified points of departure and the points of destination.
 - **token** — individual affiliate token.

### Response example
{
"prices":[{
    "value":26000.0,
    "trip_class":0,
    "show_to_affiliates":true,
    "return_date":"2016-09-18",
    "origin":"BAX",
    "number_of_changes":0,
    "gate":"AMADEUS",
    "found_at":"2016-07-28T04:57:47Z",
    "duration":null,
    "distance":3643,
    "destination":"SIP",
    "depart_date":"2016-09-09",
    "actual":true
    }],
"origins":[
	"BAX"
	],
"errors":{
	"amadeus":{
    }},
"destinations":[
	"SIP"
    ]
}

#### Response parameters
 - **origin** — the point of departure.
 - **destination** — the point of destination.
 - **show_to_affiliates** — false - all the prices, true - just the prices, found using the partner marker (recommended). The default value - true.
 - **trip_class** — the flight class:
  - **0** — the economy class,
  - **1** — the business class,
  - **2** — the first class.
 - **depart_date** — the date of departure.
 - **return_date** — the date of return.
 - **number_of_changes** — the number of transfers.
 - **value** — the cost of a flight, in the currency specified.
 - **found_at** — the time and the date, for which a ticket was found.
 - **distance** — the distance between the point of departure and the point of destination.
 - **actual** — the actuality of an offer.
 - **duration** — flight duration in minutes, taking into account direct and expectations.
 - **errors** — if the error "Some error occurred" is returned, so in this area do not have the data in the cache.
 - **gate** — the agents, which was found in the ticket.
 - **token** — individual affiliate token.

## Cheapest tickets
Returns the cheapest non-stop tickets, as well as tickets with 1 or 2 stops, for the selected route with departure/return date filters.

### Request
http://api.travelpayouts.com/v1/prices/cheap?origin=MOW&destination=HKT&depart_date=2016-11&return_date=2016-12&token=PutHereYourToken

**Important:** Old dates may be specified in a query. No error will be generated, but no data will be returned.

#### Request parameters
 - **origin** — IATA code of the departure city. IATA code is shown by uppercase letters, for example MOW.
 - **destination** — IATA code of the destination city (for all routes, enter "-"). IATA code is shown by uppercase letters, for example MOW.
 - **depart_date** *(optional)* — day or month of departure (yyyy-mm-dd or yyyy-mm).
 - **return_date** *(optional)* — day or month of return (yyyy-mm-dd or yyyy-mm).
 - **page** — optional parameter, is used to display the found data (by default the page displays 100 found prices. If the  - **destination** isn't selected, there can be more data. In this case, use the **page**, to display the next set of data, for example, page=2).
 - **token** — individual affiliate token.
 - **currency** — currency of prices. Default value is RUB.

### Response example
{
"success": true,
"data": {
	"HKT": {
		"0": {
			"price": 35443,
			"airline": "UN",
			"flight_number": 571,
			"departure_at": "2015-06-09T21:20:00Z",
			"return_at": "2015-07-15T12:40:00Z",
			"expires_at": "2015-01-08T18:30:40Z"
		},
		"1": {
			"price": 27506,
			"airline": "CX",
			"flight_number": 204,
			"departure_at": "2015-06-05T16:40:00Z",
			"return_at": "2015-06-22T12:00:00Z",
			"expires_at": "2015-01-08T18:38:45Z"
		},
		"2": {
			"price": 31914,
			"airline": "AB",
			"flight_number": 8113,
			"departure_at": "2015-06-12T13:45:00Z",
			"return_at": "2015-06-24T20:30:00Z",
			"expires_at": "2015-01-08T15:17:42Z"
		}}
	}
}


### Response parameters
 - **0, 1, 2** — sequence number in the search results.
 - **price** — ticket price (in the currency specified in the currency parameter).
 - **airline** — IATA code of the airline operating the flight.
 - **flight_number** — flight number.
 - **departure_at** — departure Date.
 - **return_at** — return Date.
 - **expires_at** — date on which the found price expires (UTC+0).
 - **token** — individual affiliate token.

## Non-stop tickets
Returns the cheapest non-stop tickets for the selected route with departure/return date filters.

### Request

http://api.travelpayouts.com/v1/prices/direct?origin=MOW&destination=LED&depart_date=2016-11&return_date=2016-12&token=PutHereYourToken

#### Request parameters
 - **origin** — IATA code of departure city. IATA code is shown by uppercase letters, for example MOW.
 - **destination** — IATA code of destination city (for all routes, enter “-”). IATA code is shown by uppercase letters, for example MOW.
 - **depart_date** *(optional)* — day or month of departure (yyyy-mm-dd or yyyy-mm).
 - **return_date** *(optional)* — day or month of return (yyyy-mm-dd or yyyy-mm).
 - **token** — individual affiliate token.
 - **currency** — currency of prices. Default value is RUB.

### Response example
{
    "success": true,
    "data": {
        "LED": {
            "0": {
                "price": 4363,
                "airline": "UT",
                "flight_number": 369,
                "departure_at": "2015-06-27T11:35:00Z",
                "return_at": "2015-07-04T16:00:00Z",
                "expires_at": "2015-01-08T20:21:46Z"
            }
        }
    }
}

#### Response parameters
 - **price** – Ticket price (in specified currency).
 - **airline** – IATA code of airline operating the flight.
 - **flight_number** – Flight number.
 - **departure_at** – Departure date.
 - **return_at** – Return date.
 - **expires_at** – Date on which the found price expires (UTC+0). 

## Tickets for each day of month
Returns the cheapest non-stop, one-stop, and two-stop flights for the selected route for each day of the selected month.

### Request

http://api.travelpayouts.com/v1/prices/calendar?depart_date=2016-11&origin=MOW&destination=BCN&calendar_type=departure_date&token=PutHereYourToken

#### Request parameters
 - **origin** — IATA code of departure city. IATA code is shown by uppercase letters, for example MOW.
 - **destination** — IATA code of destination city. IATA code is shown by uppercase letters, for example MOW.
 - **departure_date** — day or month of departure (yyyy-mm-dd or yyyy-mm).
 - **return_date** *(optional)* — day or month of return (yyyy-mm-dd or yyyy-mm). **Pay attention!** If the return_date is not specified, you will get the "One way" flights.
 - **calendar_type* — field used to build the calendar. Equal to either: departure_date or return_date
 - **length** *(optional)* — length of stay in the destination city.
 - **token** — individual affiliate token.
 - **currency** — currency of prices. Default value is RUB.

### Response example
{
    "success": true,
    "data": {
        "2015-06-01": {
            "origin": "MOW",
            "destination": "BCN",
            "price": 12449,
            "transfers": 1,
            "airline": "PS",
            "flight_number": 576,
            "departure_at": "2015-06-01T06:35:00Z",
            "return_at": "2015-07-01T13:30:00Z",
            "expires_at": "2015-01-07T12:34:14Z"
        },
        "2015-06-02": {
            "origin": "MOW",
            "destination": "BCN",
            "price": 13025,
            "transfers": 1,
            "airline": "PS",
            "flight_number": 578,
            "departure_at": "2015-06-02T17:00:00Z",
            "return_at": "2015-06-11T13:30:00Z",
            "expires_at": "2015-01-06T17:15:47Z"
        },
        ...
        "2015-06-30": {
            "origin": "MOW",
            "destination": "BCN",
            "price": 13025,
            "transfers": 1,
            "airline": "PS",
            "flight_number": 578,
            "departure_at": "2015-06-30T17:00:00Z",
            "return_at": "2015-07-23T13:30:00Z",
            "expires_at": "2015-01-07T20:15:34Z"
        }
    }
}

#### Response parameters
 - **origin** — IATA code of departure city.
 - **destination** — IATA code of destination city.
 - **price** — ticket price in the specified currency.
 - **transfers** — number of stops.
 - **airline** — IATA code of airline.
 - **flight_number** — flight number.
 - **departure_at** — departure Date.
 - **return_at** — return Date.
 - **expires_at** — when the found price expires (UTC+0). 

## Popular airline routes
Returns routes for which an airline operates flights, sorted by popularity.

### Request

http://api.travelpayouts.com/v1/airline-directions?airline_code=SU&limit=10&token=PutHereYourToken

#### Request parameters
 - **airline_code** — IATA code of airline.
 - **limit** — records limit per page. Default value is 100. Not less than 1000.
 - **token** — individual affiliate token.

### Response example
{
    "success": true,
    "data": {
        "MOW-BKK": 187491,
        "MOW-BCN": 113764,
        "MOW-PAR": 91889,
        "MOW-NYC": 77417,
        "MOW-PRG": 71449,
        "MOW-ROM": 67190,
        "MOW-TLV": 62132,
        "MOW-HKT": 58549,
        "MOW-GOI": 47341,
        "MOW-IST": 45553
    },
    "error": null,
    "currency":"rub"
}

#### Description of response

Returns a list of popular routes of an airline, sorted by popularity.


## The calendar of prices for a week
Brings the prices for the nearest dates to the target ones back.

### Request

http://api.travelpayouts.com/v2/prices/week-matrix?currency=usd&origin=LED&destination=HKT&show_to_affiliates=true&depart_date=2016-09-04&return_date=2016-09-18&token=PutHereYourToken

#### Request parameters
 - **currency** — the airline tickets currency. The default value - RUB.
 - **origin** — the point of departure. The IATA city code or the country code. The length - from 2 to 3 symbols.
 - **destination** — the point of destination. The IATA city code or the country code. The length - from 2 to 3 symbols.
 - **show_to_affiliates** — false - all the prices, true - just the prices, found using the partner marker (recommended). The default value - true.
 - **depart_date** *(optional)* — Day or month of departure (yyyy-mm-dd or yyyy-mm).
 - **return_date** *(optional)* — Day or month of return (yyyy-mm-dd or yyyy-mm).
 - **token** — individual affiliate token.

### Response example
{
    «success»:true,
    «data»:[
    {
        «show_to_affiliates»:true,
        «trip_class»:0,
        «origin»:«LED»,
        «destination»:«HKT»,
        «depart_date»:«2016-03-01»,
        «return_date»:«2016-03-15»,
        «number_of_changes»:1,
        «value»:71725,
        «found_at»:«2016-02-19T00:04:37+04:00»,
        «distance»:8015,
        «actual»:true
    }]
}

#### Response parameters
 - **origin** — the point of departure.
 - **destination** — the point of destination.
 - **show_to_affiliates** — false - all the prices, true - just the prices, found using the partner marker (recommended). The default value — true.
 - **trip_class** — the flight class:
  - **0** — the economy class,
  - **1** — the business class,
  - **2** — the first class.
 - **depart_date** — the date of departure.
 - **return_date** — the date of return.
 - **number_of_changes** — the number of transfers.
 - **value** — the cost of a flight, in the currency specified.
 - **found_at** — the time and the date, for which a ticket was found.
 - **distance** — the distance between the point of departure and the point of destination.
 - **actual** — the actuality of an offer.

## The popular directions from a city
Brings the most popular directions from a specified city back.

### Request

http://api.travelpayouts.com/v1/city-directions?origin=MOW&currency=usd&token=PutHereYourToken

#### Request parameters
 - **currency** — the airline tickets currency. The default value — RUB.
 - **origin** — the point of departure. The IATA city code or the country code. The length - from 2 to 3 symbols.
 - **token** — individual affiliate token.

### Response example
{
    "success":true,
    "data":{
        "AER":{
            "origin":"MOW",
            "destination":"AER",
            "price":3673,
            "transfers":0,
            "airline":"WZ",
            "flight_number":125,
            "departure_at":"2016-03-08T16:35:00Z",
            "return_at":"2016-03-17T16:05:00Z",
            "expires_at":"2016-02-22T09:32:44Z"
        }
    },
    "error":null,
    "currency":"rub"
}

#### Response parameters
 - **origin** — the point of departure.
 - **destination** — the point of destination.
 - **departure_at** — the date of departure.
 - **return_at** — the date of return.
 - **expires_at** — date on which the found price expires (UTC+0).
number_of_changes — the number of transfers.
 - **price** — the cost of a flight, in the currency specified.
found_at — the time and the date, for which a ticket was found.
 - **transfers** — the number of direct.
 - **airline** — IATA of airline.
 - **flight_number** — flight number.
 - **currency** — currency of response.

## Data of countries in json format
The query http://api.travelpayouts.com/data/countries.json returns a file with a list of countrys from the database. 

### Example response
[{
    "code":"NC",
    "name":"New Caledonia",
    "currency":"XPF",
    "name_translations":{
        "de":"Neukaledonien",
        "en":"New Caledonia",
        "zh-CN":"??????",
        "tr":"Yeni Kaledonya",
        "ru":"Новая Каледония",
        "fr":"Nouvelle-Caledonie",
        "es":"Nueva Caledonia",
        "it":"Nuova Caledonia",
        "th":"???????????????????"
    }}
]

#### Response parameters
 - **code** — IATA-code of country.
 - **name** — country name.
 - **currency** — currency country.
 - **name_translations** — translation of country name.

## City data in json format

The query http://api.travelpayouts.com/data/cities.json returns a file with a list of cities from the database. 

### Example response
[{
    "code":"SCE",
    "name":"State College",
    "coordinates":{
        "lon":-77.84823,
        "lat":40.85372
    },
    "time_zone":"America/New_York",
    "name_translations":{
        "de":"State College",
        "en":"State College",
        "zh-CN":"???",
        "tr":"State College",
        "ru":"Стейт Колледж",
        "it":"State College",
        "es":"State College",
        "fr":"State College",
        "th":"??????????"
    },
    "country_code":"US"
}]

#### Response parameters
 - **code** — city IATA-code.
 - **name** — city name.
 - **coordinates** — city coordinates.
 - **time_zone** — timezone relative to GMT.
 - **name_translations** — translation of city name.
 - **country_code** — country IATA-code.


## Airport data in json format
The query http://api.travelpayouts.com/data/airports.json returns a file with a list of airports from the database. 

### Example response
[{
    "code":"MQP",
    "name":"Kruger Mpumalanga International Airport",
    "coordinates":{
        "lon":31.098131,
        "lat":-25.384947
    },
    "time_zone":"Africa/Johannesburg",
    "name_translations":{
        "de":"Nelspruit",
        "en":"Kruger Mpumalanga International Airport",
        "tr":"International Airport",
        "it":"Kruger Mpumalanga",
        "fr":"Kruger Mpumalanga",
        "es":"Kruger Mpumalanga",
        "th":"???????????????"
    },
    "country_code":"ZA",
    "city_code":"NLP"
}]

#### Response parameters
 - **code** — airport IATA code.
 - **name** — airport name.
 - **lon** — airport longitude.
 - **lat** — airport latitude.
 - **time_zone** – time zone relative to GMT.
 - **name_translations** — the name of the airport in different languages.
 - **country_code** — country IATA code.
 - **city_code** — city IATA code.

## Airline data in json format
The query http://api.travelpayouts.com/data/airlines.json returns a file with a list of airlines from the database. 

### Example response
[{
    "name":"Private flight",
    "alias":null,
    "iata":null,
    "icao":null,
    "callsign":null,
    "country":null,
    "is_active":true
}]

#### Response parameters
 - **name** — airline name.
 - **alias** — alliance name (if the airline is an alliance member).
 - **iata** — airline IATA code.
 - **icao** — airline ICAO code.
 - **callsign** — airline call sign.
 - **country** — airline country of registration.
 - **is_active** — true: company is active, false: company is not active.

## Alliance data in json format
The query http://api.travelpayouts.com/data/airlines_alliances.json returns a file with a list of alliances from the database. 

### Example response
[{
    "name":"oneworld",
    "airlines":["4M","AA","AB","BA","CX","AY","HG","IB","JC","JL","JO","KA","LA","LP","MA","MN","MX","NU","QF","RJ","S7","XL"]
}]

#### Response parameters
 - **name** — alliance name.
 - **airlines** — codes for alliance member companies.

## Airplane data in json format
The query http://api.travelpayouts.com/data/planes.json returns a file with a list of airplanes from the database. 

### Example response
[{
    "code":"100",
    "name":"Fokker 100"
}]

#### Response parameters
 - **code** — plane IATA code.
 - **name** — plane name.

## Data on the routes in json format
The query http://api.travelpayouts.com/data/planes.json returns a file with a list of routes from the database.

### Example response
[{
    "airline_iata":"2B",
    "airline_icao":null,
    "departure_airport_iata":"AER",
    "departure_airport_icao":null,
    "arrival_airport_iata":"DME",
    "arrival_airport_icao":null,
    "codeshare":false,
    "transfers":0,
    "planes":[
        "CR2"
    ]
}]

#### Response parameters
 - **airline_iata** — IATA of airline.
 - **airline_icao** — ICAO of airline.
 - **departure_airport_iata** — IATA of airport of departure.
 - **departure_airport_icao** — ICAO of airport of departure.
 - **arrival_airport_iata** — IATA of airport of arrival.
 - **arrival_airport_icao** — ICAO of airport of arrival.
 - **codeshare** — it shows whether the flight performs the same company that sells the ticket.
 - **transfers** — the number of direct.
 - **planes** — IATA of airplane.

## The definition of a user's location by IP address
The query: http://www.travelpayouts.com/whereami?locale=ru&callback=useriata&ip=62.105.128.0 is returned the IATA-code and the name of nearest city from the user.

### Request Options
 - **locale** — the language in which returns the name of the city (available languages: en, ru, de, fr, it, pl, th. By default it is Russian);
 - **callback** — specifies the name of the function, which contains a response to a query (mandatory);
 - **ip** — ip-addresses of the users (if the address is not passed, the system determines IP from the header of the request).

### Example response
*useriata ({"iata": "MOW", "name": "Moscow"})*

#### Response parameters
 - **iata** — IATA code of the city where the user is located;
 - **name** — the name of the city.

## Reduction of prices into the other currency
Brings the current rate of all popular currencies to RUB back.

### Request

http://yasen.aviasales.ru/adaptors/currency.json

####Example response
{
    "cny":8.24394,
    "eur":57.1578,
    "mzn":1.49643,
    "nio":1.97342,
    "usd":51.1388,
    "hrk":7.48953
}

## Special offers
Brings the recent special offers from the airline companies back in the .XML format.

### Request
http://api.travelpayouts.com/v2/prices/special-offers
