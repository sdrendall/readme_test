# PAL API module

1. [Installation](#installation)
2. [Endpoints](#endpoints)


## Installation:
Make sure you have installed the general dev dependencies TODO LINK

Then, clone the repo:
```
git clone git@github.com:jobpal/pal-api
```

Next, install the node dependencies, (note, we have encountered issues when using `yarn` here): ```
npm install
```

And run our tests to ensure your build worked:
```
npm run test
```

##Endpoints
All the endpoints (except Utils), are single token protected, ~~the token is retrievable from the `/v1/login` endpoint~~

### Utils

-  `/v1/ping` 
GET - Used to check the app's live status

- `/v1/unauthorized` 
GET - Unauthorized request endpoint

- `/v1/memory`
GET - ????

### Offers
- `/v1/offers`
GET - Retrieves the offers for the user's group_id
    Output format - JSON:
    ```graphql
{
    total_active: INT
    total_disabled: INT
    offers: [Offer]
}
    ```

- `/v1/offers/{offer_id}`
- `/v1/offers/suggestions/cities`
- `/v1/offers/suggestions/categories`
- `/v1/offers/suggestions/levels`
- `/v1/offers/options`
- `/v1/offers/external_for_company`
- `/v1/disabled`
- `/v1/disable/{offer_id}`
- `/v1/enable/{offer_id}`
- `/v1/elastic/single/{offer_id}`
- `/v1/elastic/syncall`
- `/v1/elastic/search`
- `/v1/fboffers`
- `/v1/offersuggestions`
- `/v1/offercategorysuggestions`

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
- ``


### Applicant

* POST `/v1/login` - Login for the Applicant
    * Parameters:
        * `applicant[email] (required)`
        * `password (required)`

* GET `/v1/me/:token` - Returning the Applicant by token

* GET `/v1/me` - Returning the Applicant

* PUT `/v1/me` - Updating the Applicant data
    * Headers:
        * `Content-Type: application/x-www-form-urlencoded (required)`
        * `x-three-user-token (required)`

    * Parameters:
        * `applicant[first_name]`
        * `applicant[email]`
        * `applicant[password]`
        * `applicant[image] (Request File)`
        * `applicant[a1]`
        * `applicant[a1_link]`
        * `applicant[a2]`
        * `applicant[a2_link]`
        * `applicant[a3]`
        * `applicant[a3_link]`

* POST `/v1/me` - Creating the Applicant data
    * Parameters:
        * `applicant[first_name]`
        * `applicant[email]`
        * `applicant[password]`

* POST `/v1/me/reset/:token` - Resets the Applicant password
    * Parameters:
        * `password (required)`

* POST `/v1/forgot` - Sends the password reset link via mail to the Applicant
    * Parameters:
        * `applicant[email] (required)`

* POST `/v1/me/approver/:achievement` - Creates an Achievement (a1, a2, a3) approoval request
    * Parameters:
        * `approver - name (required)`
        * `approver - position (required)`
        * `approver - email (required)`

    * Creating an unverified aprovement request and sending a mail to the approver

### Company

* POST `/v1/users/login` - Company Login endpoint
    * Parameters:
        * `user[username] (required)`
        * `user[password] (required)`

* GET `/v1/users/me` - Returning the Company User data
    * Headers:
        * `x-three-dash-user-token (required)`

* GET `/v1/users/company` - Returning the Company data
    * Headers:
        * `x-three-dash-user-token (required)`

* GET `/v1/users/applications/new` - Returning the Company new applications
    * Headers:
        * `x-three-dash-user-token (required)`

* GET `/v1/users/applications/replied` - Returning the Company replied applications
    * Headers:
        * `x-three-dash-user-token (required)`
