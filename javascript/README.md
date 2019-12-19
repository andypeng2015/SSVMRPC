# SSVMRPC - a client-side Javascript implementation

A mechanism to deploy and execute WebAssembly(Wasm) code on secondstate.io's stateless stack-based virtual machine, located at [github.com/second-state/SSVM](https://github.com/second-state/SSVM). This Javascript code, takes JSON request objects and returns JSON response objects. 

This Javascript code is a wrapper around secondstate.io's Wasm as a service (server) application. As you will see below this application can deploy and execute Wasm remotely using only a few lines of code at most. The request that you send is received by a service provider (and server that is running secondstate.io's [SSVMRPC software](https://github.com/second-state/SSVMRPC)). The SSVMRPC software delivers your request object to secondstate.io's Wasm virtual machine(SSVM). Once SSVM has performed the execution a response object is returned to your nodejs environment.

To deploy and execute Wasm via Node.js, please see the [nodejs implementation of this code](https://raw.githubusercontent.com/second-state/SSVMRPC/master/nodejs/ssvmrpc.js). The nodejs implementation is also available on [npmjs.com/package/ssvmrpc](https://www.npmjs.com/package/ssvmrpc).
```
 <script src="/myJavascriptFiles/ssvmrpc.js"></script>
 ```
Once the js is accessible in your web page, the following line of code can be used to initialize an instance of the SSVMRPC. This line of code allows you to set the provider i.e. the SSVMRPC server that you are trying to contact.
```
var ssvmrpc = new SSVMRPC('https://ssvmrpc.secondstate.io')
```
Once the provider is set, you can then construct your [valid HTTP POST request](https://github.com/second-state/SSVMRPC/blob/master/documentation/specifications/http_post_specification.md) objects and call the functions (using syntax like `ssvmrpc.deployWasmApplication(data);`) as shown below.
```
data = {
	"request": {
		"application": {
			"bytecode": "0x01234567890",
			"name": "Application Example"
		}
	}
}

```

```
ssvmrpc.deployWasmApplication(data);
```
