# Logical Architecture

## Components

- **Okta Workforce Identity Cloud** is the identity provider (IdP), directory, authentication service, and policy decision point.
- **Universal Directory** stores the fictional workforce users Alice, Bob, and Charlie.
- **Groups** represent baseline and department access: All Employees, Finance, HR, and Engineering.
- **Finance Portal** is an Okta OIDC Single-Page Application integration.
- **Okta Verify** provides the configured MFA authenticator.
- **Default authorization server** applies the configured access policy for the Finance Portal client.
- **Okta Angular quickstart** is an official sample application used locally as an OIDC validation harness; it is not part of this repository.

## Access relationship

```text
Alice
  |
  +--> All Employees
  |
  +--> Finance
           |
           +--> Finance Portal application assignment
```

Bob and Charlie belong to All Employees plus Engineering and HR respectively. They are not in Finance and are not assigned to Finance Portal through this model.

## Authentication and OIDC flow

```text
Browser -> local Angular validation harness -> Okta authorize endpoint
                                           |
                                           v
                         Application assignment + access policy evaluation
                                           |
                                           v
                                      Okta Verify MFA
                                           |
                                           v
                  Authorization Code + PKCE -> registered callback URI
                                           |
                                           v
                          Local validation harness establishes session
```

At a high level, the browser is redirected to Okta for authentication. Okta validates that the user is assigned to the client application, evaluates the applicable access policy, and performs MFA when required. After successful authentication, the Authorization Code with PKCE flow completes at the registered redirect URI. Refresh-token capability was enabled for the client.

## Access revocation path

```text
Remove Alice from Finance
          |
          v
Finance Portal group assignment no longer applies
          |
          v
New OIDC authorization request is evaluated by Okta
          |
          v
Access denied: user is not assigned to the client application
```

Alice's access was validated before the change and denied after the Finance-group removal, demonstrating group-based deprovisioning without deleting the user identity.
