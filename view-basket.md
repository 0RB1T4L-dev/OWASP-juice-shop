## View Basket
**Category:** Broken Access Control

**Difficulty:** 2/6

**Challenge Description:** View another user's shopping basket.

## Approach

### Inspecting the HTTP request

The HTTP request which was used to retrieve the content of the own basket:

![Basket HTTP Request](/images/basket-http-request.png)

The URI /rest/basket/**6** looks a little bit suspicious. It looks like this endpoint is lacking the check if the user is requesting its own basket.

My suspicion was right, you can just retrieve everyones basket with just altering the number in the URI.

![Basket altered HTTP Request](/images/basket-altered-http-request.png)