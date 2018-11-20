# GraphQL API - Overview

Your data is 100% accessible and explorable though our GraphQL API, in fact, it's what powers the [Mbaasy App Publisher Console](https://console.mbaasy.com).

To begin exploring the data we recommend using our [GraphQL User Interface](https://console.mbaasy.com/graphql) where you will find inline documentation for the GraphQL schema. For programmatic access, please read about [Authentication](/graphql/authentication/).

![GraphQL UI](/assets/images/graphql_ui.jpg)

## Example - Querying subscriptions

This query will return the first 25 subscriptions. The *filter* argument offers many options for fine tuning your results and is what powers the filters on the *[Mbaasy App Publisher Console](https://console.mbaasy.com) > Apps > [App] > Purchases* page.

```graphql
query {
  appFamily(
    field: LOGIN_AND_NAME
    value: "[ORG_LOGIN]/[APP_FAMILY_NAME]"
  ) {
    inAppPurchasesFeed(
      filter: {
        purchaseType: SUBSCRIPTION
      }
      sort: CREATED_AT_ASC
      first: 25
    ) {
      pageInfo {
        endCursor
        hasNextPage
      }
      edges {
        node {
          userIdentifier
          productId
          currentPeriod {
            min {
              iso8601
            }
            max {
              iso8601
            }
          }
        }
      }
    }
  }
}
```

## Further Reading

Getting started with GraphQL is easy and there are some great tutorials and resources to get started with:

* [GraphQL.org](http://graphql.org/)
* [Learn GraphQL](https://learngraphql.com/)
* [Facebook GraphQL tutorial](https://facebook.github.io/relay/docs/tutorial.html)
* [Stack Overflow's GraphQL tag](https://stackoverflow.com/questions/tagged/graphql)
