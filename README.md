# This package is for [ethereum](https://ethereum.org/) dapp development

## What does this package:
 * Exports a global class named `GethConnector` available only on **server**
 * Downloads geth binaries
 * Can start geth process
 * Can connect to an ipc geth process and send  [json-rpc](https://github.com/ethereum/wiki/wiki/JSON-RPC#json-rpc-methods) call

## Example

 * On server side create a global variable just before `Meteor.startup`:
 * 
   ```javascript
	// for global access on server side
	gethObj = false;

	const testGeth = function () {
	  // start geth process
	  let started = gethObj.start();
	  // waits for process to start (Future fiber)
	  if (started) {
	    // test ipc calls https://github.com/ethereum/go-ethereum/wiki/Go-ethereum-management-API's
	    gethObj.ipcCall('personal_listAccounts', [], (err, data)=> {
	      console.log('account list ' + data);
	    });
	  }
	};

	Meteor.startup(function () {
	  gethObj = GethConnector.getInstance(); // grab singleton instance anywhere
	  gethObj.setLogLevel('warn'); // info is default
	  testGeth();
	});
   ```
   
 * Then you can use anywhere on server side these methods:
 
  ```javascript
  gethObj.start(); // logs in .meteor/local/log/gethProcess.log
  ```
  
  ```javascript
  gethObj.stop();//stop geth process
  ```

  ```javascript
  gethObj.setOptions({executable:'/usr/bin/geth', dataDir:'.ethereum/datadir', privateNet:true, testNet:false,
    extraOptions: ['--shh', '--rpc', '--rpccorsdomain', 'localhost']})
  ```

  ```javascript
  gethObj.setLogLevel('info');//info is default; 'trace', 'fine', 'debug', 'info', 'warn', 'error'
  ```
  
  ```javascript
  gethObj.ipcCall(name, params, callback));//send json-rpc request, you can find all available methods here https://github.com/ethereum/wiki/wiki/JSON-RPC#json-rpc-methods 
  ```
 * Test this package:

 ```javascript
 VELOCITY_TEST_PACKAGES=1 meteor test-packages --driver-package velocity:console-reporter akasha:meteor-geth
 ```

