| Type     | Purpose             | Ownership                   | Customers              | Senders            | Naming Style   | Naming Examples                                 |
| -------- | ------------------- | --------------------------- | ---------------------- | ------------------ | -------------- | ----------------------------------------------- |
| Command  | Invoke Behavior     | Owned by Consumer           | One                    | Many               | Verb           | `CreateInvoice`, `CancelOrder`, `ShipItem`      |
| Event    | Something Happened  | Owned by Publisher          | Zero or Many           | Single Publisher   | Past Tense     | `InvoiceCreated`, `OrderCancelled`              |
| Query    | Request Information | Owned by Consumer           | One                    | One                | Question-style | `GetInvoiceById`, `FindOrdersByCustomer`        |
| Response | Return Information  | Owned by Sender (Responder) | One (the Query Issuer) | One (Query Target) | Noun/DTO name  | `Invoice`, `OrderListResult`, `CustomerDetails` |
