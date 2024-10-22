# Node.js Demonstrating Proof-of-Possession (DPoP) in a Service App Example

This repository contains a working example of a DPoP enabled service app used to connect to Okta management API. Please read [How to Build Secure Okta Node.js Integrations with DPoP][blog] for a detailed guide through.

## Prerequisites

* [Node.js](https://nodejs.org/en) v18 or greater
* IDE (I used [VS Code](https://code.visualstudio.com/))
* Terminal window (I used the integrated terminal in VS Code)
* An Okta org

> [Okta](https://developer.okta.com/) has Authentication and User Management APIs that reduce development time with instant-on, scalable user infrastructure. Okta's intuitive API and expert support make it easy for developers to authenticate, manage and secure users and roles in any application.

## Setup the project

To run this example, run the following commands:

```bash
git clone https://github.com/oktadev/okta-node-dpop-example.git
cd okta-node-dpop-example
npm ci
```

### Set up the Okta application

* Create an `API service app` and save *Client ID*
* Change app *Client authentication* to `Public Key / Private Key`
* Add a key in *PUBLIC KEYS* section and save private key (PEM format) in the following path in the project `assets/cc_private_key.pem` and click *Save*
* In *General Settings* section, edit *Proof of possesion* > *Require Demonstrating Proof of Possession (DPoP) header in token requests* to `true`
* Under *Okta API Scopes* tab, grant `okta.users.read` scope
* Under *Admin Roles* tab, assign `Read-only Administrator`


### Generate DPoP signing keys

We will need a new keypair to sign DPoP proof JWTs. If you know how to generate one, feel free to skip this step. I used the following steps to generate it.
  * Go to [JWK generator](https://mkjwk.org/)
  * Select the following and then click Generate.
    * Key Use: Signature
    * Algorithm: RS256
    * Key ID: SHA-256
    * Show X.509: Yes
  * Copy the Public Key (json format) and save it to `assets/dpop_public_key.json`
  * Copy the Private Key (X.509 PEM format) (**Do not click Copy to Clipboard. This will copy as single line which will not work with this following steps. Instead copy value manually and save it**) and save it to `assets/dpop_private_key.pem`

### Define OAuth 2.0 configuration 

Create `.env` file in project root directory and fill with the following:

```
OKTA_ORG_URL=https://{yourOktaDomain}
OKTA_CLIENT_ID={yourClientID}
OKTA_SCOPES=okta.users.read
OKTA_CC_PRIVATE_KEY_FILE=./assets/cc_private_key.pem
OKTA_DPOP_PRIVATE_KEY_FILE=./assets/dpop_private_key.pem
OKTA_DPOP_PUBLIC_KEY_FILE=./assets/dpop_public_key.json
```

## Run the project

Run the project using `npm start`. You can switch branches to run a sample with and without DPoP enabled.

## Help

Please post any questions as comments on the [blog post][blog], or visit our [Okta Developer Forums](https://devforum.okta.com/).

## License

Apache 2.0, see [LICENSE](LICENSE).

[blog]: https://developer.okta.com/blog/2024/10/23/dpop-oauth-node
