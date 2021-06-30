# System Health Monitoring Api

Author:  Gregg Lind <gregg.lind@...>
Date:  2021-06-30

## System Goals

Transparency, action, prediction (later)

- Identify problem layer.
- Identify what to do to repair (point of contact).
- enable "watch" and monitoring services

### Beliefs and Assumptions Affecting Design
- Low-volume api
- Outages are uncommon for any particular API 
- Failures don't compound / cascade much.  A "fix" probably fixes everything.

## System Diagram

|           Sources          	|      Storage / Api     	|          Consumers         	|
|:--------------------------:	|:----------------------:	|:--------------------------:	|
| Fun Response Dashboard 	    |                        	| Fun Response Dashboard 	|
|   3rd Party Status Pages   	| Fun Health Service 	    |      Email / PagerDuty     	|
|     Proactive Heartbeat    	|      (RDB Storage)     	|    Fun Dev Dashboard   	|
|  (other incident sources)  	|          (API)         	|   Embeddable Site Widget   	|
|                            	|                        	|                            	|

Sources publish to the Storage / API using `POST` endpoints
Consumers read from the Storage / API using `GET` endpoints

### Service Model

For each Service, in each Region

A service can be 

OK
Slow
Down

An underlying CAUSE (e.g., AWS East outage) can affect multiple Services.

An ISSUE is a service impacting message.  

## Api 

### Overview (Tbd)
* Endpoint  `/api` 
* Auth:  (auth details)
* Demo Url  `demo.example.com/api`
* Example Usage

```js
// authorize

// GET open incidents

// POST and update

// For all outages affecting my app

```

## Description

`/issues/`

Get Queries:

`POST /issue/{id}/q=details`  create a new issue 
```
{
    id:   (optional, null if create)
    service:  string (e.g., slack)
    source:  string source (manual, hb, status page)
    status:  enum (OK, slow, down),
    details:  (TBD: future narrative or action fields)
    regions
}
```

`GET /overview/me` Overview to power in-site 'up' widget
`GET /issues/open` unsolved problems
`GET /issues/me`   issues affecting me
`GET /issues/updates` issues recently updated
`GET /issues/?q=region`  (other simple queries)
`GET /service/{name}` recent history for service(s)

```
{
    service:  string name
    status:  
    narrative:
    respsonse:
}
```

`@restricted GET /service/{name}/customers` customers names and userbase affected by "service"


##  Prior Art

- api availability
- incident response
- bug tracking system

https://apimetrics.readme.io/docs
https://apimetrics.io/api-outages/
https://www.atlassian.com/software/statuspage
https://developer.statuspage.io


## Term / Glossary:

Provider:  3rd party (Slack, SAP)
Fun:  Integration Health Provider.  Because Outages are *fun*.


## To be decided and discussed

Handling Service by Region?  Is by Region necessary?
"Meta incidents" that affect many services (AWS issues, for example, or other CDN things)
