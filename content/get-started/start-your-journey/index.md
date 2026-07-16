-ReportingCloud Docs
}
Search
Introduction 
 What is ReportingCloud?
 The Idea New
 Work with your Trial Account
 Creating API Keys
 Create your First Document
 Portal Overview
 Getting Support
Quickstart Tutorials 
 .NET Core Quickstart New
 .NET Framework Quickstart
 PHP Quickstart
API Reference 
 Overview
 Creating Documents
 Managing Templates
 Managing Account
 Font Information
 Proofing
 Processing
Concepts 
 Merging Templates
 The Merge Data JSON Object
 Templates with JSON Excerpt Files
 Nested Repeating Blocks
Typical Tasks 
 Setting the Culture for Date and Currency Fields
 Merging Images into Image Placeholders
 Conditional Text Blocks Based on Merge Blocks
 Merge HTML Content into Merge Fields
ReportingCloud SDKs 
 SDK Overview
 .NET Framework SDK (C#)
 ReportingCloud PHP SDK
 ReportingCloud Java SDK
 ReportingCloud Ruby SDK
/v1/document/merge/
https://api.reporting.cloud/v1/document/merge

Merges and returns a compatible template from the template storage or an uploaded template with hierarchical JSON data.

Document Quota
This method counts against the document quota. For each successful request, the quota count is increased by 1.

Authorization
This endpoint requires a "ReportingCloud-APIKey" or a "Basic" user authorization to access the user acount, data and templates. Only one of these two methods are required.


Request Parameters
Query Parameter	Value Type	Description
returnFormat	String	A string that specifies the format of the created document. Possible values are: PDF, PDFA, RTF, DOC, DOCX, HTML and TX.
templateName	String	Optional. The name of the template in the template storage. If no template is specified, the template must be uploaded in the MergeBody object of this request.
append	Boolean	Optional. Specifies whether the documents should be appened to one resulting document when more than 1 data row is passed.
test	Boolean	Optional. Specifies whether it is a test run or not. A test run is not counted against the quota and created documents contain a watermark. Not possible using the Free or Trial license.
Request Payload
Value Type	Description
MergeBody	The MergeBody object contains the datasource as a JSON data object and optionally, a template encoded as a Base64 string and a ReportingCloud MergeSettings object.
MergeBody
Key	Value Type	Description
mergeData	JSON object	The datasource for the merge process as a JSON array.
template	Base64 encoded string	Optional. The template encoded as a Base64 string. Supported formats are RTF, DOC, DOCX and TX.
mergeSettings	ReportingCloud MergeSettings object	Optional. Optional merge settings to specify merge properties and document properties such as title and author.
MergeSettings
Key	Value Type	Description
removeEmptyFields	Boolean	Optional. Specifies whether empty fields should be removed from the template or not. The default value is true.
removeEmptyBlocks	Boolean	Optional. Specifies whether the content of empty merge blocks should be removed from the template or not. The default value is true.
removeEmptyImages	Boolean	Optional. Specifies whether images which don't have merge data should be removed from the template or not. The default value is false.
removeEmptyLines	Boolean	Optional. Specifies whether lines should be removed that contain only empty fields and no other content. The default value is false.
removeTrailingWhitespace	Boolean	Optional. Specifies whether trailing whitespace should be removed before saving a document. The default value is true.
mergeHtml	Boolean	Optional. Specifies whether field data can contain formatted Html content or not. The default value is false. Html content must be enclosed in an tag element. Only active in the Merge endpoint.
author	String	Optional. Sets the document's author.
creationDate	DateTime (String)	Optional. Sets the document's creation date which will be saved in the document.
lastModificationDate	DateTime (String)	Optional. Sets the date the document is last modified.
creatorApplication	String	Optional. Sets the application, which has created the document.
documentSubject	String	Optional. Sets the document's subject string which will be saved in the document. PDF limitation: The length is limited to 2000 characters.
documentTitle	String	Optional. Sets the document's title that will be saved in the document. PDF limitation: The length is limited to 2000 characters.
userPassword	String	Optional. Specifies the password for the user to open the document.
culture	String	Optional. Specifies the culture for the merge process for date and currency values. It must be the Language Culture Name that can be found in this list. For French use "fr-FR", for German "de-DE". Default is English "en-US".
Success Response
Return Value	Description
200 (OK)	On success, the HTTP status code in the response header is 200 (OK). The response body contains an array of the created documents encoded as Base64 encoded strings.
Error Response
Return Value	Description
403 (Forbidden)	A 403 (Forbidden) is returned, if the user is not authorized, the document quota is exceeded or more than 2 concurrent requests came in from the same account.
400 (Bad Request)	A 400 (Bad Request) is returned, if no data is found in the MergeBody object, no template is uploaded or template is not found in the template storage.
Timeout Limitation
The pure processing time is limited to a maximum of 60 seconds. After this period of time, the task is cancelled.

A typical document should not take more than 5 seconds. If your requests takes longer, please decrease the number of data rows and reduce the template size.

Sample Requests
Http
.NET
Java
Ruby
PHP
Request:

<?php

use TxTextControl\ReportingCloud\ReportingCloud;

$reportingCloud = new ReportingCloud([
    'api_key' => Helper::apiKey(),
]);

$mergeData = [
    0 => [
        'yourcompany_companyname' => 'Text Control, LLC',
        'yourcompany_zip' => '28226',
        'yourcompany_city' => 'Charlotte',
        'yourcompany_street' => '6926 Shannon Willow Rd, Suite 400',
        'yourcompany_phone' => '704 544 7445',
        'yourcompany_fax' => '704-542-0936',
        'yourcompany_url' => 'www.textcontrol.com',
        'yourcompany_email' => 'sales@textcontrol.com',
        'invoice_no' => '778723',
        'billto_name' => 'Joey Montana',
        'billto_companyname' => 'Montana, LLC',
        'billto_customerid' => '123',
        'billto_zip' => '27878',
        'billto_city' => 'Charlotte',
        'billto_street' => '1 Washington Dr',
        'billto_phone' => '887 267 3356',
        'payment_due' => '20/1/2016',
        'payment_terms' => 'NET 30',
        'salesperson_name' => 'Mark Frontier',
        'delivery_date' => '20/1/2016',
        'delivery_method' => 'Ground',
        'delivery_method_terms' => 'NET 30',
        'recipient_name' => 'Joey Montana',
        'recipient_companyname' => 'Montana, LLC',
        'recipient_zip' => '27878',
        'recipient_city' => 'Charlotte',
        'recipient_street' => '1 Washington Dr',
        'recipient_phone' => '887 267 3356',
        'item' => [
            0 => [
                'qty' => '1',
                'item_no' => '1',
                'item_description' => 'Item description 1',
                'item_unitprice' => '2663',
                'item_discount' => '20',
                'item_total' => '2130.40',
            ],
            1 => [
                'qty' => '1',
                'item_no' => '2',
                'item_description' => 'Item description 2',
                'item_unitprice' => '5543',
                'item_discount' => '0',
                'item_total' => '5543',
            ],
        ],
        'total_discount' => '532.60',
        'total_sub' => '7673.4',
        'total_tax' => '537.138',
        'total' => '8210.538',
    ],
];

// copy data 4 times
// total record sets = 5

for ($i = 0; $i < 5; $i++) {
    array_push($mergeData, $mergeData[0]);
}

$mergeSettings = [

    'creation_date'              => time(),
    'last_modification_date'     => time(),

    'remove_empty_blocks'        => true,
    'remove_empty_fields'        => true,
    'remove_empty_images'        => true,
    'remove_trailing_whitespace' => true,

    'author'                     => 'James Henry Trotter',
    'creator_application'        => 'The Giant Peach',
    'document_subject'           => 'The Old Green Grasshopper',
    'document_title'             => 'James and the Giant Peach',

    'user_password'              => '123456789',
];

$templateName = 'test_template.tx';

$arrayOfBinaryData = $reportingCloud->mergeDocument($mergeData, 'PDF', $templateName, null, false, $mergeSettings);

foreach ($arrayOfBinaryData as $index => $binaryData) {

    $destinationFile     = sprintf('test_document_%d.pdf', $index);
    $destinationFilename = sys_get_temp_dir() . DIRECTORY_SEPARATOR . $destinationFile;

    file_put_contents($destinationFilename, $binaryData);

    var_dump("Merged {$templateName} was written to {$destinationFilename}");
}
Results:

string(63) "Merged test_template.tx was written to /tmp/test_document_0.pdf"
string(63) "Merged test_template.tx was written to /tmp/test_document_1.pdf"
string(63) "Merged test_template.tx was written to /tmp/test_document_2.pdf"
string(63) "Merged test_template.tx was written to /tmp/test_document_3.pdf"
string(63) "Merged test_template.tx was written to /tmp/test_document_4.pdf"
Post

Authorization
Request Parameters
Request Payload
MergeBody
MergeSettings
Success Response
Error Response
Sample Requests
Copyright © 2026 Text Control GmbH and Text Control, LLC. Happy Coding! :-)


Text Control, ReportingCloud and certain product names used herein maybe trademarks or registered trademarks of Text Control GmbH and/or one of its subsidiaries or affiliates in the U.S. and/or other countries.
Text Control Privacy Policy | Imprint
