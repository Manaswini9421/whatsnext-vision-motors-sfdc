# whatsnext-vision-motors-sfdc

Salesforce DX project for **WhatsNext Vision Motors** — a vehicle sales and
service CRM built on Salesforce, covering the data model, automation, and
Apex logic behind the ordering, test-drive, and service-request process.

## Data model

| Object | Purpose |
|---|---|
| `Vehicle__c` | Vehicle inventory (model, price, stock, dealer) |
| `Vehicle_Dealer__c` | Authorized dealer locations |
| `Vehicle_Customer__c` | Customer records |
| `Vehicle_Order__c` | Vehicle purchase orders |
| `Vehicle_Test_Drive__c` | Test drive bookings |
| `Vehicle_Service_Request__c` | Post-sale service requests |

Custom tabs are defined for all six objects and grouped under the
**WhatsNext Vision Motors** Lightning App (`force-app/main/default/applications`).

## Automation

- **Auto Assign Dealer** (`force-app/main/default/flows`) — record-triggered
  flow on `Vehicle_Order__c`: when a new order is created with `Status__c =
  Pending`, it looks up the ordering customer, matches them to the nearest
  dealer by location, and assigns that dealer.

## Apex

- `VehicleOrderTriggerHandler` / `VehicleOrderTrigger` — blocks orders for
  out-of-stock vehicles and decrements stock when an order is confirmed.
- `VehicleOrderBatch` — batch job that re-checks pending orders and confirms
  them once stock is available.
- `VehicleOrderBatchScheduler` — schedules `VehicleOrderBatch` to run nightly
  via `System.schedule`.

## Project structure

```
force-app/main/default/
├── applications/   # Lightning App (App Manager)
├── classes/        # Apex classes (trigger handler, batch, scheduler)
├── flows/          # Auto Assign Dealer flow
├── objects/        # Custom objects + their fields
├── tabs/           # Custom tabs for each object
└── triggers/       # VehicleOrderTrigger
```
