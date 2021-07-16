# eservice.pl-API-Specification-documentation-Specyfikacja-dokumentacja
eservice.pl API Specification documentation Specyfikacja dokumentacja The purpose of this document is to provide a description of Intelligent Payments’ gateway API, and is specifically intended for merchant integration purposes. Instrukcja obsługi 

# keys
AUTH/PURCHASEAPI Operation – Hosted Payment Page Integration
6.1 Session Token Request
6.1.1 Format
POST Request
6.1.2 Definition
Parameter Data Type Mandatory Description
Security Data
Mandatory to identify the merchant in the eService Gateway
merchantId Integer (18) Y The identifier for the merchant in the eService Gateway provided at on-boarding
password String (64) Y The merchant’s password in the eService Gateway provided at on-boarding
Transaction Data
The Transaction Data defines the type of transaction the merchant is requesting the eService Gateway to perform, how the transaction result will be managed, and
complimentary data required by the Authentication and Authorisation Processes. The transaction result can be the Authentication or Authorisation response.
action String
(enum) Y Must be “AUTH” or “PURCHASE”
firstTimeTransaction Boolean N
A flag to indicate if the transaction is the customer’s first. This affects the behaviour of the 3D
Secure processing.
Note: if a customerId value is not provided, the eService Gatewaywill always treat the transaction
as a first-time transaction for the customer
timestamp Integer (13) Y Milliseconds since 1970-01-01 00:00:00
channel String
(enum) Y
The transaction channel through which the payment was taken:
“ECOM”for card present e-commerce type transactions that are customer initiated, usually
through a website checkout screen
country
String
(enum) Y
The ISO alpha-2 code country in which the transaction takes place, as defined in the ISO 3166
standard
If this is not known or unavailable, the customerAddressCountry will be used.
allowOriginUrl String (256) Y
The merchant's URL that will make the Auth/Purchase (see Section 6.4)
This will usually be the URL of the customer’s browser.
Cross-Origin Resource Sharing (CORS) headers will allow only this origin
API Operations Specification
Version 2.5 Page 18 of 61 25.01.2021
Parameter Data Type Mandatory Description
merchantNotificationUrl String (200) Y
The merchant’s server-to-server communications URL, to which the Transaction Result Call will
be sent.
It is highly recommended that this parameter is provided, so that the merchant receives a timely
result of the payment authentication and authorisation in the Transaction Result Call.
If not provided, no immediate notification will be sent to the merchant. The transaction result
will be shown in the eService Gateway Back-Office or it can be retrieved using the GET STATUS
API Operation.
merchantLandingPageUrl String (200) N The URL to which the customer’s browser is redirected for success or failure messaging
Payment Method Data
The Payment Method Data defines how the merchant’s customer wishes to pay for an Authorisation or Purchase (action = ‘AUTH’ or ‘PURCHASE’)
paymentSolutionId Integer (18) N
The eService Gateway Payment Solution Identifier
See section 12 - GET AVAILABLE PAYMENT SOLUTIONS API Operation for valid values
Merchant Transaction Data
Merchant Transaction Data provides information about the merchant’s bank account, information needed to recognise the merchant in the acquirer and settlement
systems, and data that the merchant wants to add to the transaction for post settlement reconciliation and processing.
merchantTxId String (50) N
The merchant’s reference for the transaction. If the parameter is empty or omitted, a reference
will be generated by the eService Gateway as a hexadecimal string, and returned in the
transaction responses
It is highly recommended that a value is supplied to reconcile transactions in the eService
Gatewaywith the merchant’s own order management system
operatorId String (20) N
Identifier of the merchant’s operator or agent on behalf of the end customer, if the operation is
not performed by the merchant, and the merchant wants to track the operator who performed
the transaction
brandId Integer (18) N
The eService Gateway Brand Id for the merchant’s goods or services supplied at on-boarding
If not provided the merchant’s default eService Gateway Brand Id will be used
freeText String (200) N
A free text field for use by the merchant that is returned in the Transaction Result Call (see 10 -
Transaction Result Call)
customParameter1Or …
customParameter20Or String (50) N
20 Text Fields that used by merchants to reconcile transactions performed through mobile
applications with results from the acquirer.
s_text1, s_text2... s_text5 String (200) N 5 Text fields for general use
d_date1, d_date2... d_date5 Date/Time N
5 Date fields for general use.
Format: DD/MM/YYYY hh:mm:ss – the time part can be omitted, resulting in 00:00:00
b_bool1, b_bool2... b_bool5 Boolean N 5 Boolean fields for general use – accepted values are "true" and "false"
n_num1, n_num2... n_num5 BigDecimal
(7.2) N
5 Numeric fields for general use – a dot “.” must be used as a decimal separator, not the comma
"," and a thousand separator must not be used
API Operations Specification
Version 2.5 Page 19 of 61 25.01.2021
Parameter Data Type Mandatory Description
Customer Browser/App/Device Data
The Customer Browser/App/Device Data is required to support Strong Customer Authentication (SCA) and 3DS V2.x when an Authentication Challenge (3DS) is
required.
Although the parameters are non-mandatory in the initial release, as much information should be supplied as is available. This will enable card issuers to provide
more Frictionless Flows in the Authentication processes, where the cardholder is not challenged during the transaction.
userDevice String
(enum) C
Type of device used, accepted values:
 “MOBILE”
 “DESKTOP”
Condition: Required for 3DSV2.x. If not supplied, 3DSV1.0 Authentication will be used
userAgent String (1024) C
Browser User-Agent: Exact content of the HTTP user-agent header from the browser in which
the transaction was performed
Note: If the total length of the User-Agent sent by the browser exceeds 2048 characters, the
excess content will be truncated.
Conditions: Required for 3DSV2.x. If not supplied, 3DSV1.0 Authentication will be used
customerIPAddress String (45) C
Browser IP Address: IP address of the customer’s browser, where the transaction is initiated, as
returned by the HTTP headers to the merchant
Value accepted:
 IPv4 address is represented in the dotted decimal format of 4 sets of decimal numbers
separated by dots. The decimal number in each and every set is in the range 0 to 255.
Example IPv4 address: 1.12.123.255
 IPv6 address is represented as eight groups of four hexadecimal digits, each group
representing 16 bits (two octets. The groups are separated by colons (:). Example IPv6
address:2011:0db8:85a3:0101:0101:8a2e:0370:7334
Condition: Required for 3DSV2.x unless market or regional mandate restricts sending this
information.
language String
(enum) N
The ISO alpha-2 language code, as defined in ISO 639-1 standard, for the language to be used in
the Hosted Payment Page, when loaded to the merchant’s webpage.
 If a supported language code is provided, the language translation will be provided
 If not provided or an unsupported language code is provided, the merchant’s default
language is used
[Please consult your eCommerce Support Team for currently supported languages]
Transaction Amount Data
Transaction Amount Data provides the values of the sale
