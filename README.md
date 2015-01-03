# comodojo/xmlrpc

Yet another php xmlrpc decoder/encoder.

Main features:

- support for <nil /> and <ex:nil />
- implements true, XML compliant, HTML numeric entities conversion
- support for CDATA values

# Usage

## Encoding request/response

Create an encoder instance:

	```php
	
	// create an encoder instance
	$encoder = new \Comodojo\Xmlrpc\XmlrpcEncoder();

	// (optional) set character encoding
	$encoder->setEncoding("utf-8");

	// (optional) use ex:nil instead of nil
	$encoder->useExNil();

	// (optional) declare special types in $data
	$encoder->setValueType($data['a_value'], "base64");
	$encoder->setValueType($data['b_value'], "datetime");
	$encoder->setValueType($data['c_value'], "cdata");
	
	// Wrap actions in a try/catch block (see below)
	try {

		// encoder actions

	} catch (\Comodojo\Exception\XmlrpcException) {

		// someting goes wrong during encoding

	} catch (\Exception) {
		
		// generic error

	}

	```

Single call:

	```php
	
	$call = $encoder->encodeCall("my.method", array("user"=>"john", "pass" => "doe")) ;

	```

Multicall:

	```php
	
	$multicall = $encoder->encodeMulticall( array (
		"my.method" => array( "user"=>"john", "pass" => "doe" ),
		"another.method" => array( "value"=>"foo", "param" => "doe" ),
	);

	```

Single call success response

	```php
	
	$response = $encoder->encodeResponse( array("success"=>true) );

	```

Single call error response

	```php
	
	$error = $encoder->encodeError( 300, "Invalid parameters" );

	```

Multicall success/error (faultString and faultCode should be explicitly declared in $data)

	```php
	
	$values = $encoder->encodeResponse( array(

		array("success"=>true),

		array("faultCode"=>300, "faultString"=>"Invalid parameters")

	);

	```

## Decoding 

Create a decoder instance:

	```php
	
	// create a decoder instance
	$decoder = new \Comodojo\Xmlrpc\XmlrpcDecoder();
	
	// Wrap actions in a try/catch block (see below)
	try {

		// decoder actions

	} catch (\Comodojo\Exception\XmlrpcException) {

		// someting goes wrong during decoding

	}

	```

Decode single call

	```php
	
	$incoming_call = $decoder->decodeCall( $xml_singlecall_data );

	```

Decode multicall

	```php
	
	$incoming_multicall = $decoder->decodeMulticall( $xml_multicall_data );

	```

Decode response
	
	```php
	
	$returned_data = $decoder->decodeResponse( $xml_response_data );

	```
