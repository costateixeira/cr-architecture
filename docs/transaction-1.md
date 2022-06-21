---
title: T1. PUT FHIR Bundle
parent: Transactions
grand_parent: Specifications
nav_order: 1
---


## Transaction 1 - PUT FHIR Bundle


> FHIR IDs are set by the client. 

> We use a PUT method so that if message need to resent or replayed, resource just get upserted rather than recreated. This helps the client not care so much about bundles/resources that have already been sent and it makes it easier to replay messages at any point in the flow in case of failures.
