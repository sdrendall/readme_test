# PAL API module

1. [Installation](#installation)
2. [Endpoints](#endpoints)
	- [Offers](#offers)
	- [Companies](#companies)
	- [Applications](#applications)
	- [Utils](#utils)
3. [Updating the Docs](#updating-the-docs)

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
All endpoints (except Utils), are single token protected, ~~the token is retrievable from the `/v1/login` endpoint~~. Each endpoint returns JSON of the following form:

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

---
---
### Offers

API endpoints corresponding to job offers.

---
#### Index

- [`/v1/offers`](#v1offers)
- [`/v1/offers/{offer_id}`](#v1offersoffer_id)
- [`/v1/offers/suggestions/cities`](#v1offerssuggestionscities)
- [`/v1/offers/suggestions/categories`](#v1offerssuggestionscategories)
- [`/v1/offers/suggestions/levels`](#v1offerssuggestionslevels)
- [`/v1/offers/options`](#v1offersoptions)
- [`/v1/offers/external_for_company`](#v1offersexternal_for_company)
- [`/v1/offers/disabled`](#v1offersdisabled)
- [`/v1/disable/{offer_id}`](#v1disableoffer_id)
- [`/v1/enable/{offer_id}`](#v1enableoffer_id)
- [`/v1/elastic/single/{offer_id}`](#v1elasticsingleoffer_id)
- [`/v1/elastic/syncall`](#v1elasticsyncall)
- [`/v1/elastic/search`](#v1elasticsearch)
- [`/v1/fboffers`](#v1fboffers)
- [`/v1/offersuggestions`](#v1offersuggestions)
- [`/v1/offercategorysuggestions`](#v1offercategorysuggestions)


---
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

`ko_status`:

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

##### Response Format:

```graphql
{
    offer: Offer
}
```

##### Error Modes:

`ko_status`:

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

`ko_status`:

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

`ko_status`:

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

`ko_status`:

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

`ko_status`:

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

`ko_status`:

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

---
---
### Companies

Endpoints for company data

---
#### Index

- [`/v1/companies`](#v1companies)
- [`/v1/companies/{group_id}`](#v1companiesgroup_id)
- [`/v1/companies/{group_id}/invite-users`](#v1companiesgroup_idinvite-users)
- [`/v1/company/{group_id}/applications`](#v1companygroup_idapplications)
- [`/v1/company/{group_id}/offers`](#v1companygroup_idoffers)
- [`/v1/company/{group_id}`](#v1companygroup_id)
- [`/v1/company/{group_id}/{offer_id}`](#v1companygroup_idoffer_id)
- [`/v1/company/{group_id}/email`](#v1companygroup_idemail)
- [`/v1/company/{company_id}/faqnotificationemail`](#v1companycompany_idfaqnotificationemail)

---
#### `/v1/companies`

**GET** :: retrieve paginated company data

##### Response Format:
```graphql
[Company]
```

Note: a next link is returned when a page of company data is returned that does not contain all companies

##### Error Modes:

`ko_status`:

- Unhandled company model error

**POST** :: Inserts a new company into the database

##### Request Body Format:

XXX - There are many company fields that are not used in updateCompany. Are they ignored?

```graphql
{
    name: ?String
    social_1: ?String
    social_2: ?String
    social_3: ?String
    description: ?String
    features: {
        offer: Feature
        applications: Feature
        faq: Feature
    }
}
```

##### Response Format:
```graphql
{
    name: ?String
    social_1: ?String
    social_2: ?String
    social_3: ?String
    description: ?String
    features: {
        offer: Feature
        applications: Feature
        faq: Feature
    }
}
```

##### Error Modes:

`ko_status`:

- Unhandled error saving company
- Company already exists
- Unhandled company model error


---
#### `/v1/companies/{group_id}`

**DELETE** :: Deletes the company specified by `group_id`. All documents corresponding to `group_id` are dropped from the following models: `company`, `user`, `offer`, `applicant`, `application`

##### Response Format:
```graphql
''
```

##### Error Modes:

`ko_status`: 

- Unhandled model error


---
#### `/v1/companies/{group_id}/invite-users`

**POST** :: Creates users for the specified `group_id` and sends out invitation links.

##### Request Body Format:
```graphql
{
    emails: [String]
}
```

##### Response Format:
```graphql
{
    data: [User || "ko_email_exists"]
}
```

##### Error Modes:

`ko_unknown`:

- Unhandled company model error


---
#### `/v1/company/{group_id}/applications`

**GET** :: Retrieves the applications corresponding to a specific `group_id`.

##### Response Format:
```graphql
{
    applications: [Application]
}
```

`ko_status`: 

- Unhandled application model error


---
#### `/v1/company/{group_id}/offers`

**GET** :: Retrieves the offers corresponding to a specific `group_id`

##### Response Format:
```graphql
[Offer]
```

Note: A next link is provided if pagination is used.

**POST** :: Saves a set of offers for a given company

##### Request Body Format:
```graphql
{
    _id: !OfferID
    group_id: !CompanyID
    title: String!
    description: String!
    link: String!
    city: String!
    channel: String
    country: String
    description: String
    division: String
    unit: String
    type: String
    link: String
    level: Number
    category: String
    status: ?Boolean
}
```

XXX - level data type

##### Response Format:
```graphql
{
    group_id: String
    offer_id: String
    status: Boolean
    company: [Company]
    level: [Number]
    category: String
    title: String
    slug: String
    full_slug: String
    channel: String
    city: String
    unit: String
    type: String
    country: String
    description: String
    questions: [String]
    p1: String
    p2: String
    p3: String
    link: String
    created: DateTime
    updated: DateTime
    applications: {}
    applications_count: Number
    is_multiple: Number
}
```

XXX - applications data type

Error Modes:

`ko_status`:

- Company not found
- Unhandled offer model error


---
#### `/v1/company/{group_id}`

**GET** :: Retrieves the company specified by `group_id`

##### Response Format:
```graphql
[Company]
```

Error Modes: 

`ko_status`:

- Unhandled company model error

**POST** :: Updates the company corresponding to `group_id`

##### Request Body Format:
```graphql
{
    name: ?String
    social_1: ?String
    social_2: ?String
    social_3: ?String
    description: ?String
}
```

##### Response Format:
```graphql
{
    name: ?String
    social_1: ?String
    social_2: ?String
    social_3: ?String
    description: ?String
}
```

Error Modes:

`ko_status`:

- Error saving company
- Unhandled company model error


---
#### `/v1/company/{group_id}/{offer_id}`

**GET** :: Retrieves the offer corresponding to `group_id` and `offer_id`

##### Response Format:
```graphql
[Offer]
```

Error Modes:

`ko_status`:

- Offer not found
- Company not found
- Unhandled offer model error


---
#### `/v1/company/{group_id}/email`

**POST** :: Sends a notification email to each user at the company corresponding to `group_id`

##### Request Body Format:
```graphql
{
    subject: String!
    message: String!
}
```

##### Response Format:
```graphql
''
```

Error Modes:

`ko_status`:

- Incomplete request body
- Company not found (invalid `group_id`)
- Unhandled company model error


---
#### `/v1/company/{company_id}/faqnotificationemail`

**POST** :: alias for [`/v1/company/{group_id}/email`](#v1companygroup_idemail)

---
---
### Applications

Endpoints for job applications

---
#### Index

- [`/v1/applications/new`](#v1applicationsnew)
- [`/v1/{offer_id}/applications/new`](#v1offer_idapplicationsnew)
- [`/v1/applications/replied`](#v1applicationsreplied)
- [`/v1/{offer_id}/applications/replied`](#v1offer_idapplicationsreplied)
- [`/v1/application/offer/{offer_id}`](#v1applicationofferoffer_id)
- [`/v1/application/reply/{application_id}`](#v1applicationreplyapplication_id)
- [`/v1/application/{application_id}`](#v1applicationapplication_id)

---
#### `/v1/applications/new`

**GET** :: Retrieves all of the new applications for the current user.

##### Response Format:
```graphql
{
    total_new: Integer
    total_replied: Integer
    applications: [Application]
}
```

##### Error Modes:

`ko_status`:

    - Unhandled user model error
    - Unhandled company model error


---
#### `/v1/{offer_id}/applications/new`

**GET** :: Retrieves all of the new applications for the current user corresponding to a specific `offer_id`. Considered for deprecation.

##### Response Format:
```graphql
{
    total_new: Integer
    total_replied: Integer
    applications: [Application]
}
```

##### Error Modes:

`ko_status`:

- Unhandled user model error
- Unhandled company model error

---
#### `/v1/applications/replied`

**GET** :: Retrieves all applications for the current user that have been replied to.

##### Response Format:
```graphql
{
    total_new: Integer
    total_replied: Integer
    applications: [Application]
}
```

##### Error Modes:

`ko_status`:

- Unhandled user model error
- Unhandled company model error


---
#### `/v1/{offer_id}/applications/replied`

**GET** :: Retrieves all applications for the current user and corresponding `offer_id` that have been replied to. Considered for deprecation.

##### Response Format:
```graphql
{
    total_new: Integer
    total_replied: Integer
    applications: [Application]
}
```

##### Error Modes:

`ko_status`:

- Unhandled user model error
- Unhandled company model error


---
#### `/v1/application/offer/{offer_id}`

**POST** :: Creates an application for the offer corresponding to `offer_id`

##### Request Body Format:
```graphql
{
    applicant: {
        first_name: String!
        email: String!
        a1: String!
        a1_link: String
        a2: String!
        a2_link: String
        a3: String!
        a3_link: String
        linkedin_link: String
        website_link: String
    }!
    screening_dialog: [{
        question: String!
        answer: String!
    }]
    source: String
    cv: {
        link: String
        filename: String
    }
    custom_integration: String
    q1: String
    q2: String
    q3: String
}
```


---
#### `/v1/application/reply/{application_id}`

**GET** :: Sets the replied status of the application specified by `application_id` to `true`

##### Response Format:
```graphql
[Application]
```

##### Error Modes:

`ko_status`:

- Unhandled application model error

---
#### `/v1/application/{application_id}`

**GET** :: Retrieves the application specified by `application_id` 

##### Response Format:
```graphql
{
    application: Application
}
```

---
---
### Utils

####  `/v1/ping` 
**GET** :: Used to check the app's live status

#### `/v1/unauthorized` 
**GET** :: Unauthorized request endpoint

#### `/v1/memory`
**GET** :: ????

## Updating the docs

### Creating a new endpoint

1. Add documentation for the endpoint to this readme under the appropriate section (offers, companies, applications etc.). See the [template](#template) for the proper formatting.
2. Add a link to the section's index. To create a link to the endpoint header, remove all non-alphanumeric characters other than underscores and dashes, and lowercase the header. For example, for a header that looks like this:
```markdown
#### `/v1/some/stupidHeader/with-lots/{of_annoying}/characters.jpg`
```
The link code would look like this:
```
[`/v1/some/stupidHeader/with-lots/{of_annoying}/characters.jpg`](#v1somestupidheaderwith-lotsof_annoyingcharacters.jpg)
```

[`/v1/some/stupidHeader/with-lots/{of_annoying}/characters.jpg`](#v1somestupidheaderwith-lotsof_annoyingcharacters.jpg)

#### `/v1/some/stupidHeader/with-lots/{of_annoying}/characters.jpg`

### Template

Here is a template to use for new endpoints. Replace the filler data with the info appropriate for your endpoint. I'm using graphql syntax so that type annotations have nice highlighting. Check their [docs](https://graphql.org/learn/schema/) for a reference on standard types and syntax.

**Note:** Creating a markdown code block in markdown is tricky... i had to indent this to make it format properly. please remove the indent before using this template

```markdown

    ---
    #### `/some/endpoint/url`

    **GET** :: <brief description of behavior>

    ##### Response Format:
    ```graphql
    {
        user: {
            name: String
            birthday: DateTime
            children: [User]
        }
    }
    ```

    ##### Error Modes:

    `ko_status`:

    - Error Mode 1
    - Error Mode 2

    **POST** :: <brief description of behavior>

    ##### Request Body Format:

    ```graphql
    {}
    ```

    ##### Response Format:
    ```graphql
    {}
    ```

    ##### Error Modes:

    `ko_status`:

    - Error Mode 1
    - Error Mode 2

```
