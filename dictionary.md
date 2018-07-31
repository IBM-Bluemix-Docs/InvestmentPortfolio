---

copyright:
  years: 2018
lastupdated: "2018-07-31"

---
{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:codeblock: .codeblock}
{:screen: .screen}
{:pre: .pre}


# Defining portfolios and holdings
{: #data_dictionary_investportshort}

When you specify portfolios and holdings for use with 
the {{site.data.keyword.investportshort}} service, you can refer to these tables.
{:shortdesc}

**Attention**: Don't include personal data (for example, 
personal names, email addresses, or telephone numbers) when you 
specify details for portfolios or holdings.


## Defining a portfolio
Portfolios are collections of financial securities that are held at a 
certain point in time. Portfolios are specified as an array of objects 
that are defined by these properties:

|Property Name|Type|Constraint|
|-------------|----|----------|
|name|string|required|
|timestamp|string|required|
|closed|integer|optional|
|data|object|optional|
|\_rev|string|optional|
{: caption="Table 1. Properties of a portfolio" caption-side="top"}

<!-- Exampes of contents for the data object
"strategy":"emerging markets", 
"sub-strategy":"distressed debt",  
"socially_responsible":false,
-->
You can use the optional data object of a portfolio to define one or more 
characteristics of a portfolio itself for downstream analysis.  The data object is open. Specify the characteristics that you need.
The table shows examples of some commonly used fields in the portfolio data object.

|Property Name|Type|Constraint|
|------------|----|----------|
|base_currency|string|optional|
|benchmark|string|optional|
|socially\_responsible|boolean|optional|
|strategy|string|optional|
|sub\-strategy|string|optional|
|type|string|optional|
{: caption="Table 2. Available properties of a data object" caption-side="top"}



### Example: Defining and loading a single portfolio

The following example shows how you can define a portfolio with a data object:

```
{
  "name": "Balanced",
  "timestamp": "2018-04-20T13:51:56.830Z",
  "closed": false,
  "data": {
    "base_currency": "USD", 
    "benchmark": "S&P500", 
    "socially_responsible":false, 
    "strategy":"emerging markets", 
    "sub-strategy":"distressed debt"
  }
}
```
{:codeblock}

The following example shows a request to create the portfolio according to the definition:

```
curl -X POST \
  -u "<service-user-id>":"<service-user-password>" \
  --header 'Content-Type: application/json' \
  --header 'Accept: application/json' \
  -d '{ "name":"Balanced", "timestamp": "2018-04-20T13:51:56.830Z", "closed": false, "data": { "base_currency": "USD", "benchmark": "S&P500", "socially_responsible":false, "strategy":"emerging markets", "sub-strategy":"distressed debt" }}' \
  <service-url>/api/v1/portfolios
```
{:codeblock}

### Example: Defining and loading multiple portfolios

The following example shows how to define three portfolios that share names but have different time stamp values:

```
{
  "portfolios": [
    {
      "name": "Aggressive",
      "timestamp": "2018-04-19T00:00:01.000Z"
    },
    {
      "name": "Aggressive",
      "timestamp": "2018-04-19T01:00:02.000Z"
    },
    {
      "name": "Aggressive",
      "timestamp": "2018-04-19T01:30:03.000Z"
    }
  ]
}
```
{:codeblock}

The following example shows a request to create the three portfolios:

```
curl -X POST \
  -u "{service-user-id}":"{service-user_password}" \
  --header 'Content-Type: application/json' \
  --header 'accept: application/json' \
  -d '{"portfolios": [{"name": "Aggressive","timestamp": "2018-04-19T00:00:01.000Z"},{"name": "Aggressive","timestamp": "2018-04-19T01:00:02.000Z"},{"name": "Aggressive","timestamp": "2018-04-19T01:30:03.000Z"}]}' \
    https://<service-url>/api/v1/bulk_portfolios
```
{:codeblock}


## Defining holidings
The individual constituents of a portfolio of financial securities are 
referred to as _holdings_. Holdings are defined as an array of one or 
more Holdings objects. Holdings are associated with a time stamp. A 
portfolio's holdings can change over time as securities are bought, 
sold, expire, mature, or otherwise enter or leave the portfolio.

When you define a holding, the asset property is the primary 
identifier. You can assign more fields, which serve as metadata, to 
the holding for downstream analysis and aggregation. The table 
shows properties that are commonly used to describe a holding.

|Property Name|Type|Constraint|
|-------------|----|----------|
|instrumentID|string|optional|
|asset|string|required|
|name|string|optional|
|quantity|integer|optional|
|currency|string|optional|
|price|float|optional|
|id|string|optional|
{: caption="Table 3. Properties of a holding" caption-side="top"}

### Example: Defining and uploading multiple holdings

The following example shows how to define several holdings for a specific time:

```
		{
			"timestamp": "2018-04-18T13:31:00.000Z",
			"holdings": [
				{
				"instrumentId": "CX_US64110LAM81_USD", 
				"Asset Class": "Corporate",
				"name": "NETFLIX INC 144A",
				"PRICE": "USD",
				"CURRENCY": "",
				"quantity": 1000
				}
				,
				{
				"instrumentId": "CX_US61166WAP68_USD", 
				"Asset Class": "Corporate",
				"name": "MONSANTO COMPANY",
				"PRICE": "USD",
				"CURRENCY": "",
				"quantity": 2000
				}
				,
				{
				"instrumentId": "CX_US00206RCR12_USD", 
				"Asset Class": "Corporate",
				"name": "AT&T INC",
				"PRICE": "USD",
				"CURRENCY": "",
				"quantity": 500
				}
				,
				{
				"instrumentId": "CX_US7415034039_NSQ", 
				"Asset Class": "Equities",
				"name": "PRICELINE GROUP INC/THE",
				"PRICE": "USD",
				"CURRENCY": "",
				"quantity": 100
				}
			]
		}
```
{:codeblock}

The following example shows a request to create the holdings for the specified portfolio named P1:

```
curl -X POST \
-u "{service-user-id}":"{service-user_password}" \
--header 'Content-Type: application/json' \
--header 'Accept:application/json' \
-d '{"timestamp": "2018-04-18T13:31:00.000Z","holdings": [{"instrumentId": "CX_US64110LAM81_USD", "Asset Class": "Corporate","name": "NETFLIX INC 144A","PRICE": "USD","CURRENCY": "","quantity": 1000},{"instrumentId": "CX_US61166WAP68_USD", "Asset Class": "Corporate","name": "MONSANTO COMPANY","PRICE": "USD","CURRENCY": "","quantity": 2000},{"instrumentId": "CX_US00206RCR12_USD", "Asset Class": "Corporate","name": "AT&T INC","PRICE": "USD","CURRENCY": "","quantity": 500},{"instrumentId": "CX_US7415034039_NSQ", "Asset Class": "Equities","name": "PRICELINE GROUP INC/THE","PRICE": "USD","CURRENCY": "","quantity": 100}]}' \
{service-url}/api/v1/portfolios/P1/holdings
```
{:codeblock}
