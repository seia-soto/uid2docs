[UID2 Overview](./README.md) > Supply-Side Workflow

# Supply-Side Workflow Overview

The following steps provide a high-level outline of the workflow intended for organizations that propagate UID2 tokens to the bid stream via SSPs—for example, identity providers, publishers, and SSO providers. Publishers can choose to work with an SSO provider or an independent ID provider that is interoperable with UID2. The latter can handle the UID2 integration on behalf of publishers.

1. A user visits a publisher website, mobile app, or CTV app.
2. The publisher explains the value exchange of the open internet and requests the user to log in.
3. Once the user logs in, the publisher sends the first-party PII and corresponding privacy settings to the UID2 Operator via an SDK or direct API integration. A publisher may authorize an SSO provider or identity provider to pass PII and privacy settings on their behalf.
4. The UID2 Operator performs the salt, hash, and encryption process and returns the UID2 Token.
5. The publisher stores the UID2 Token to share with SSPs during real-time bidding.
   - Server-side: The publisher stores the token in a mapping table, DMP, data lake, or other server-side application.
   - Client-side: The publisher stores the token in a client-side app or in the user’s browser as a first-party cookie.
6. The publisher sends the UID2 token to the SSP at the time of impression.
7. The SSP places a bid request using the UID2 token, capturing it in the bid stream.
8. The publisher requests updated UID2 tokens using a refresh token. When applicable, the refresh token includes a user’s opt-out request.

![Publisher Workflow](/images/publisher_workflow.jpg)


## Integrations 

For integration scenarios, token management, and other details, see [Publisher Integration Guides](/api/v2/guides/README.md). See also [Endpoints](/api/v2/endpoints/README.md).

### Direct Integration Requirements

Publishers who want to send users' PII and generate UID2s must meet the followign requirements:

- Have access the UID2 Operator API.
- Integrate with UID2 Operator API to generate UID2 tokens.
- Maintain refresh tokens or use the JavaScript client-side SDK provided by UID2 to manage the refresh token.
- Enable sending the UID2 token to SSPs and other integrating organizations.

### Integration Through SSO or Identity Providers

Publishers may choose to work with an SSO or independent ID provider who is interoperable with UID2. The provider may handle the UID2 integration on their behalf.

