## Marketing Analytics Case Study

Creating personalized customer emails based off marketing analytics. Will be helping the customer analytics team generate individualized data to populate their email campaign.

### Requirement #1
#### Top two categories

For each customer, identify two top categories based off thier past rental history.

### Requirement #2
#### Category Film Recommendations

Acquire three popular film for each customer's top two categories. The films cannot be a film already seen by the customer. If three films are not available, at least show one film. 

Customers that do not have any film recommendations for either category must be flagged out and excluded from the email campaign - high level of importance. 

### Requirement #3,4
#### Individual Customer Insights

The number of films watched by each customer in their top 2 categories is required as well as the following:

    For the 1st category

        1. How many total films have they watched in their top category?
        2. How many more films has the customer watched compared to the average DVD Rental Co customer?
        3. How does the customer rank in terms of the top X% compared to all other customers in this film category?

    For the second ranking category (requirement 4):

        1. How many total films has the customer watched in this category?
        2. What proportion of each customer’s total films watched does this count make?

Note the specific rounding of the percentages with 0 decimal places!

### Requirement #5
#### Favorite Actor Recommendations

Including top actor film recommendations where three more films are included as well as the count of films by the top actor. Any films that have already been recommended in the top two categories must be excluded in the actor recommendation section. 

If no recommendation exists, exclude this section from the email campaign. 

### ERD Diagrams using dbdiagram.io

Example on how to create them
```
Table "rental" {
  "rental_id" integer [not null]
  "rental_date" timestamp [not null]
  "inventory_id" integer [not null]
  "customer_id" smallint [not null]
  "return_date" timestamp
  "staff_id" smallint [not null]
  "last_update" timestamp [not null, default: `now()`]

Indexes {
  inventory_id [type: btree, name: "idx_fk_inventory_id"]
  (rental_date, inventory_id, customer_id) [type: btree, unique, name: "idx_unq_rental_rental_date_inventory_id_customer_id"]
}
}

Table "inventory" {
  "inventory_id" integer [not null, default: `nextval('inventory_inventory_id_seq'::regclass)`]
  "film_id" smallint [not null]
  "store_id" smallint [not null]
  "last_update" timestamp [not null, default: `now()`]

Indexes {
  (store_id, film_id) [type: btree, name: "idx_store_id_film_id"]
}
}
```
#### Embedding interactive ERD

In a markdown document, the following code can be used to generate ERD using iframe
```
<iframe height="400" width="100%"
src='https://dbdiagram.io/embed/5fe1cb6e9a6c525a03bbf839'>
</iframe>

```







