# ml-mosip-integration

This repository contains all the artifacts to support the mojaloop MOSIP workstream work. 
This initiative aims to demonstrate how three digital public goods platforms namely Mojaloop, MOSIP and MIFOS can together support a superior solution for the G2P use case. This workstream's plan is to build out a proof of concept to test the design that has been proposed.

# Design Summary

The design involved the creation of a Beneficiary Registration Portal which is called by a beneficiary management system using an Open ID connect flow. (Open ID Connect is a standard way that software solutions call a central user authorization login.)
If the registration is successful, then a JTW token is returned that contains the payment token to be used in future Mojaloop payments. 

Here is a diagram detailing the solution.
![Architecture](/Docs/mosip-poc-PayeeIdentifierMapping.png)

The beneficiary registration portal make use of the MOSIP (MISP) Open ID connect redirect portals to identify the user. The MISP is an e-signet + MOSIP combination that supports a redirect login for a person / party allowing them to login to MOSIP using their personal details. The user must then select an existing Mojaloop Party Identifier as a means of selecting where the payment should be made to. The party identifier is verified by the portal by making a Mojaloop Discovery call which returns some identifiable information. It is possible that a Mojaloop Scheme can be structured to support various levels of personal identifiable data that is returned. At a minimum it includes the persons name, but optionally it can include full-name, date of birth and any number of other KYC information e.g. a MOSIP UIN number. The portal then verifies that the parties are the same by comparing the returned personal data with the personal data that is made available from MOSIP. If the validation process is successful, then a payment token is generated and registered at both Mojaloop and the DFSP. This POC generalizes the DFSP interaction by building an adapter that sits between the Mojaloop Connector (SDK Scheme Adapter) and the Core banking system connector to support this functionality. This adapter can be applied to all existing DFSP integrations that make use of the Mojaloop Connector (SDK Scheme Adapter).

## DFSP Token Adapter

The user _payment token_ mapping will be stored in the DFSP token adapter (in-memory cache for POC). The adapter connects the Mojaloop Connector (SDK Scheme Adapter) to the Core-Connector. All messages are passed through directly except for Alias party token identifiers. These identifiers are replaced with the mapped identifiers that were registered against the token. The returned results are mapped in reverse. 

## Sequence Diagrams
Here are detailed sequence diagrams for this POC design.

### Party Authentication & Validation
Next the beneficiary must authenticate themselves with MOSIP and select a payment identifier. The identifier is validated and personal details are validated between MOSIP and Mojaloop DFSP.
![Authentication](/Docs/G2PTokenRegistrationRedirect-2-Authentication.png)

### Token Registration
It is now time to create the payment token and register it.
![Token Registration](/Docs/G2PTokenRegistrationRedirect-3-TokenRegistration.png)

### Mojaloop Payment (P2P)
This shows how the new Payment Token can be use in a standard Mojaloop P2P transfer.

![Token Registration](/Docs/G2PTokenRegistrationRedirect-4-P2PIntegration.png)

### Token Adapter
This shows a detailed integration with the DFSP payment token adapter

![Token Adapter Details](/Docs/TokenAdapterDetails.png)
