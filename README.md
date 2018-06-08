# PAL API module

1. [Installation](#installation)
2. [Endpoints](#endpoints)
	- [General Output Format](#general-output-format)
	- [Companies](#companies)
	- [Applications](#applications)
	- [Applicant](#applicant)
	- [Utils](#utils)

## Installation:
Make sure you have installed the general dev dependencies TODO LINK

Then, clone the repo:
```
git clone git@github.com:jobpal/pal-api
```

Next, install the node dependencies, (note, we have encountered issues when using `yarn` here): 
```
npm install
```

And run our tests to ensure your build worked:
```
npm run test
```

## Endpoints
All the endpoints (except Utils), are single token protected, ~~the token is retrievable from the `/v1/login` endpoint~~

### General Output Format
All endpoints return JSON in the following form:

```graphql
{
	status: {
		value: StatusCode
		message: String
	} 
	response: Data || ""
  	links: ?{
		next: URL
	}
}
```

The format of the response field is specific to individual endpoints, and can be found with their corresponding documentation.

### Offers

API endpoint corresponding to job offers

#### `/v1/offers`
**GET** :: Retrieves all offers corresponding to the user's `group_id`

##### Response format:
    
```graphql
{
    total_active: Int
    total_disabled: Int
    offers: [Offer]
}
```

---
#### `/v1/offers/{offer_id}`

**GET** :: Retrieves the offers matching the specified `offer_id`

##### Response Format:

(response is an array, NOT an object)

```graphql
[Offer]
```

##### Error Modes:

`formatNotallowed`:

- invalid `offer_id`

**POST** :: Retrieves the offers for the user's `group_id`

##### Request Body Format:

```graphql
{
	offer: {
	    level
	    category
	    title
	    city
	    division
	    unit
	    type
	    country
	    description
	    link
	    questions
	}
}
```

Response Format:

```graphql
{
    offer: Offer
}
```

##### Error Modes:

`formatNotallowed`:

- Invalid question format
- Unhandled user model error
- Unhandled offer model error


---
#### `/v1/offers/suggestions/cities`
    
**GET** :: Retrieves a list of names of cities containing the most job offers

##### Response Format:

XXX - returns an array called `top_city_names` generated with cities.map(x => x._id)

```graphql
[String]
```


---
#### `/v1/offers/suggestions/categories`

**GET** :: Retrieves a list of names of categories containing the most job offers

##### Response Format:

    XXX - returns an array called `top_category_names` generated with categories.map(x => x._id)

```graphql
[String]
```


---
#### `/v1/offers/suggestions/levels`

**GET** :: Retrieves a list of names of position levels containing the most job offers

##### Response Format:

    XXX - returns an array called `top_level_names` generated with levels.map(x => x._id)

```graphql
[String]
```


---
#### `/v1/offers/options`

**GET** :: Retrieves the aggregated metadata for all offers in available to a user

##### Response Format:

```graphql
{
    cities: [{
        name: String
        country: String
    }]
    levels: [
        {name: 'Internship'}
        {name: 'Entry Level'}
        {name: 'Mid Level'}
        {name: 'Senior Level'}
    ]
    categories: [
        {name: String}
    ]
    skills: [SkillTag]
}
```

##### Error Modes:

`formatNotallowed`:

- Unhandled offer model error


---
#### `/v1/offers/external_for_company`

DEPRECATED


---
#### `/v1/offers/disabled`

**GET** :: Retrieves the offers that the user has disabled

##### Response Format:

```graphql
{
    total_active: Int
    total_disabled: Int
    offers: [Offer]
}
```

##### Error Modes:

`formatNotallowed`:

- Unhandled user model error
- Unhandled offer model error


---
#### `/v1/disable/{offer_id}`

**GET** :: Disables the offer specified by `offer_id`

##### Response Format:

```graphql
{
    total_active: Int
    total_disabled: Int
    offers: [Offer]
}
```

##### Error Modes:

`formatNotallowed`:

- Unhandled user model error
- Unhandled offer model error


---
#### `/v1/enable/{offer_id}`

**GET** :: Enables the offer specified by `offer_id`

##### Response Format:

```graphql
{
    total_active: Int
    total_disabled: Int
    offers: [Offer]
}
```

##### Error Modes:

`formatNotallowed`:

- Unhandled user model error
- Unhandled offer model error


---
#### `/v1/elastic/single/{offer_id}`

**POST** :: Uploads an offer to elastic search

##### Request Body Format:

Offer is resolved from the `offer_id` provided in the url

```graphql
{}
```

##### Response Format:
```graphql
[ElasticSearchResponse]
```

##### Error Modes:

`formatNotallowed`:

- Unhandled offer model error


---
#### `/v1/elastic/syncall`

**POST** :: Performs a full resync for elasticsearch

##### Request Body Format:

```graphql
{}
```

##### Response Format:
```graphql
[]
```

##### Error Modes:

`ko_status`:

- Error on fullResync


---
#### `/v1/elastic/search`

**GET** :: Gets a list of offer search results

##### Response Format:
```graphql
[OfferID]
```


---
#### `/v1/fboffers`

XXX - not sure what this does


---
#### `/v1/offersuggestions`

**GET** :: Retrieves cities containing the most offers and the offer categories found in those cities

XXX - not sure how this differs from offercategorysuggestions

##### Response Format:
```graphql
[
    {
        city: City
        categories: [String]
    }
]
```


---
#### `/v1/offercategorysuggestions`

**GET** :: Similar to offersuggestions

XXX ask Andriy for difference

##### Response Format:
```graphql
[
    {
        city: City
        categories: [String]
    }
]
```


### Companies
- `/v1/companies`
- `/v1/companies/{group_id}`
- `/v1/companies/{group_id}/invite-users`
- `/v1/company/{group_id}/applications`
- `/v1/company/{company_id}/offers`
- `/v1/company/{group_id}/offers`
- `/v1/company/{company_id}`
- `/v1/company/{company_id}/{offer_id}`
- `/v1/company/{group_id}`
- `/v1/company/{company_id}/email`
- `/v1/company/{company_id}/faqnotificationemail`

### Applications
- `/{offer_id}/applications/new`
- `/applications/new`
- `/{offer_id}/applications/replied`
- `/applications/replied`
- `/application/offer/{offer_id}`
- `/application/reply/{application_id}`

### Utils

####  `/v1/ping` 
**GET** :: Used to check the app's live status

#### `/v1/unauthorized` 
**GET** :: Unauthorized request endpoint

#### `/v1/memory`
**GET** :: ????
