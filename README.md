# Smooth Deployments

[Stream URL](https://stack-stream.com/case/dont-disturb-your-users-smooth-deployments)

## Why is this important?
- Goal for user: Do action he/she wants to do in the application or buy a product
- If this is disturbed or interrupted by an update user might not feel satisfied 
- -> Might result in user abort action/buying or buy less
- But: Evaluate cost-benefit ratio for your improvements!

## Infrastructure and Whole system

### Rolling Deployments
If possible in the infrastructure/environment, create new release and gracefully replace new releases with old ones.

Example:
- Use central entrypoint like gateway or load-balancer to forward request to correct target service
- Create new deployments for new versions of your service
- Wait for the new deployments to be ready
- Central entrypoint will forward request to new deployments
- Old deployments/services will be terminated gracefully

### Improve Resilience
Resilience describes the ability to provide a working system, handle problems gracefully and provide self-healing functionality to the system.

- Add retry/timeout mechanism, e.g. for network requests, database connections, messaging
- Add fallback logic
- Have loose coupling between services
- (Switch to more stable or higher available infrastructure components)

### Configuration and Feature flags

By providing a configuration, system administration or by using feature flags you can first deploy your code and enable a feature in the available tool. If problems occur this might also be used to disable the feature and fix the problems.

## Frontend/Client

When opening a website or webapp the application is usually fetched from the server. However, due to the nature of AJAX/Fetch the site might run in the browser (client side). This can result in layout and functionality problems when deploying a new release.

- Implement frontend update mechanism: Might be much work!
- Adjust/reduce "Cache-Control" header for frontend assets
- Use file hashing to identify changes and trigger loading new files

Hashing example:
- Without hashes: *main.js*, *product.js*, *profile.js*; Not possible to identify if there is a change in the file when referencing these files
- With hashes, before update: *main-abc123.js*, *product-456d78.js* , *profile-923e43.js*
- With hashes, after update: *main-abc123.js*, *product-456d78.js* , *profile-777a88.js*; main and product have same hash and did not change, profile has new hash and should be fetched from server.

## Handle Breaking Changes 

Breaking changes might result in downtime if more than one service/component is deployed.

Example for applying smooth breaking change releases:
- Example: Change **freeShipping** boolean field (true/false) to get more information about shipping conditions.
- Backend: Add new field **freeShippingConditions**, deprecated field **freeShipping**
- Frontend: Use **freeShippingConditions** field, maybe ask/force users to update
- Backend: Maybe wait some time, remove deprecated **freeShipping** field
  