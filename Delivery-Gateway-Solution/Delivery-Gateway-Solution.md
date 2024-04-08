# Delivery Gateway Solution
This solution is for a Delivery Gateway Solution. 

## Problem Statement
An eCommerce Business needs a workflow for amanging customers interfaction and order after the point of sale. Currently the interaction with the customer is handed over to their external delivery partner, and the business relies on a update later to know if the delivery was successful.

## Proposed Solution
<img src="https://www.planttext.com/api/plantuml/png/hLF1Ri8m3BtdAtmSsH1enqfbK8hGTXXey0DUQerga6H9AjQ_NvfsqB0UTuai-_dPVa-PMuV6LQSZOze7fIWrIWSBhO-bG5Sg6UNNwEYCzL0kpvOe3iHoTFYEbgvvnbpfZWGB2QjlF6aVRfLri3oG-5ILWuqfoC2Bgeoa6iQmew-Af238I_mmU69f2gzqxd1x1PbdC4gGjLH0chBUkkDPDpS1phURtPvpc4WwXzuxrRpF1Tp3WT27TiGwE8ndSEdKBNde6G3aMGBMAKCHwOKgpoAoDx9QIKk_PL_hxNkjmvttmbKMX2eeWSVfzC6Vq95pFcfKQ1NOjRYW7zpS7usUXEuM7j1F7TtWrPolLurSlQO_jqKg1CCJy6QPEiG3KeJyV-lHBjTeSZidDWbjBGLKkl2flZi3HqHGDYTP-4_n0m00" width="700" height="700"/>

So this diagram is split into two sections:

1. New Order To Be Processed
- The Application calls the Delivery Gateway Endpoint (ideally this should be a POST call if we are being truly RESTful.) 
- The Delivery Gateway Endpoint will be processing the order information with the necessary systems.
- Once successfully completed, the order will be stored in the GatewayDB for future retrival.
- The Successfully stored order is then passed back to the Application with the orderId. 

2. Order Status Check (This step already assumes the order has already been processed).
- The Application will call the Delivery Gateway Endpoint (Ideally this should be a GET call to be RESTful.)
- The Delivery Gateway will then process the request for the order status.
- The Delivery Gateway will then fetch the order status from the Gateway Database Cache which should be read-optimised, and asynchronously updated.

## Suggested Technology

Database: AuroraDB
GatewayDBCache: ElasticCache (Redis)
Gateway: API Gateway

**Optional:** You may also choose to use an Lambda as the Application layers, as this solution has been designed to be low latency, so it would be perfect for a short lived compute service such as an AWS Lambda, which can then send the respond to an upstream system. 
