Royal Mail Stamp Barcodes
--

In 2022 Royal Mail introduced barcodes on all postage stamps and retired all non-barcoded stamps in July 2023.

The barcodes look like a 2D Type 29 Mailmark barcode, but the data cannot be parsed using the [specification for these codes](https://www.royalmail.com/sites/royalmail.com/files/2023-09/mailmark-barcode-definition-document-september-2023.pdf).

Either these stamps or the web service used to retrieve data about them appears to internally be referred to as "PennyBlack".

There are two lengths of code matched by the following regular expressions:
| Length | Regex | 
| ------ | ----- |
| 70     | `(JGB)\\s[A-Z0-9]{1}[A-Z0-9]{1}[A-Z0-9]{1}[A-Z0-9]{1}[A-Z0-9]{1}[0-9]{7}[0-9]{8}[0-9]{5}[0-9]{6}[A-Z0-9]{2}[A-Z0-9\\s]{9}[A-Z0-9\\s]{6}[A-Z0-9\\s]{16}[A-Z0-9\\s]{2}` |
| 55     | `(JGB)\\s[A-Z0-9]{1}[A-Z0-9]{1}[A-Z0-9]{1}[A-Z0-9]{1}[A-Z0-9]{1}[0-9]{7}[0-9]{8}[0-9]{5}[0-9]{6}[A-Z0-9]{2}[A-Z0-9\\s]{18}` |

The data format we understand so far is as follows:

| Field Name | Definition | Start Position | Length | Legitimate Value(s) | Comment | 
| ---------- | ---------- | ---------------|------- | ------------------- | ------- |
| UPU Country ID | Identifies the UPU Country ID | 1 | 4 | "JGB<space>" | n/a |
| Information Type ID | Identifies the Royal Mail Mailmark barcode payload for each product type. | 5 | 1 | "S" | S, for stamp. We think. |
| Stamp Type | ? | 6 | 1 | "1" | We have only seen "1" in the wild. |
| Mystery Bytes | No idea | 7 | 2 | | No idea what these mean. |
| Supply Chain ID | Identifies the unique group of customers involved in the mailing | 9 | 7 | "0000000" to "9999999" | Format is Numeric only. |
| Item ID | Identifies the unique item within the Supply Chain ID. | 16 | 8 | "00000000" to "99999999" | This is basically the stamp ID, it is one digit longer than a normal Mailmark Item ID. Every Mailmark barcode is required to carry an ID so it can be uniquely identified for at least 90 days. Format is Numeric only. |
| Week Of Production | The week the stamp was produced. | 29 | 6 | "250322" | Fairly sure this is in DDMMYY format. |
| Campaign | ? | 35 | 2 | "01" | Possibly special christmas stamps etc? We have only seen "01". |
