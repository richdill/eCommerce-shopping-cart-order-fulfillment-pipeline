This pipeline processes an eCommerce shopping cart through a sequential workflow that includes stock availability checking, stock reservation with ship date confirmation, payment processing, order placement, and final order queuing to JMS. The pipeline handles a shopping cart containing hardware items (brass doorlock, key, striker plate) and processes it through multiple REST API calls to different services, with error handling at each stage.

Snap Summary:

Shopping cart input : generates XML content containing a shopping cart with three items - brass doorlock, key, and striker plate with their respective product IDs, descriptions, prices, and quantities.
Check stock ATP : makes a REST API call to check stock availability by posting the XML shopping cart data with Bearer token authentication.
Map content : maps the response entity from the ATP check to a content field for further processing.
Document to Binary : converts the document to binary format using DOCUMENT codec.
XML Parser: parses the binary XML content back into structured data format.
reserve stock CSD : makes a REST API call to confirm ship date and reserve stock by posting the cart data with JSON content type and Bearer token authentication.
JSON Formatter: formats the data into JSON structure.
write CSD error : writes any errors from the ship date confirmation process to a file named "csdPostErrors.json".
Map content : maps the response entity and sets content-type to application/xml for the ship date confirmation response.
Document to Binary : converts document to binary format using NONE codec.
XML Parser: parses the XML response from ship date confirmation.
post pay order : makes a REST API call to process payment for the order using Bearer token authentication.
JSON Formatter: formats the payment response data into JSON structure.
write pay error : writes any payment processing errors to "PayPostError.json" file.
Map content : maps the payment response entity and sets content-type to application/xml.
Document to Binary : converts payment response document to binary format.
XML Parser: parses the XML payment response.
post order placed : makes a REST API call to confirm order placement with Bearer token authentication.
JSON Formatter: formats the order placement response into JSON structure.
write PO error : writes any order placement errors to "POError.json" file.
Map content : maps the order placement response entity and sets content-type to application/xml.
Document to Binary : converts order placement response to binary format.
XML Parser: parses the XML order placement response.
write order to JMS Queue : makes a final REST API call to write the processed order to a JMS queue with XML content type.
JSON Formatter: formats the final JMS response into JSON structure.
write JMS error : writes any JMS queue errors to "JMSPostError.json" file.
