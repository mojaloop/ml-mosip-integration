# ml-mosip-integration

This repository contains all the artifacts to support the mojaloop MOSIP workstream work. 
This initiative aims to demonstrate how three digital public goods platforms namely Mojaloop, MOSIP and MIFOS can together support a superior solution for the G2P use case. This workstream's plan is to build out a proof of concept to test the design that has been proposed.

# Design Summary

The design involved the creation of a Beneficiary Registration Portal which is called by a beneficiary management system using an Open ID connect flow. (Open ID Connect is a standard way that software solutions call a central user authorization login.)
If the registration is successfully, then a JTW token is returned that contains the payment token to be used in Mojaloop payments. 

Here is a diagram detailing the solution.
![Architecture](/Docs/mosip-poc-architecture.png)

The beneficiary registration portal make use of two other Open ID connect redirect portals. The first is a MOSIP locking using e-signet, the second is the DFSP's party login portal. These are used to validate the user and their access to the destination account to support the selection of an account. A token is then generated and is registered at both Mojaloop and the DFSP.

## MIFOS Integration

The mifos integration start with some pre-configurations. These are specializations to help manage the token registration.

### Pre configurations
![Pre configurations](/Docs/G2PTokenRegistrationRedirect-1-Preconfig.png)


### Authentication
Next the beneficiary must authenticate themselves and their DFSP account. This involved authentication against MOSIP and Mifos.
![Authentication](/Docs/G2PTokenRegistrationRedirect-2-Authentication.png)

### Token Registration
Next the token must be generated and registered.
![Token Registration](/Docs/G2PTokenRegistrationRedirect-3-TokenRegistration.png)

### Mojaloop Payment (P2P)
This view show how this information means that a standard P2P can be used to make the payment.

![Token Registration](/Docs/G2PTokenRegistrationRedirect-4-P2PIntegration.png)
