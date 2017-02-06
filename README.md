# Ethereum-android : Easy-to-use Ethereum client wrapper for Android 

[![Build Status](https://travis-ci.org/sqli-nantes/ethereum-android.svg?branch=master)](https://travis-ci.org/sqli-nantes/ethereum-android)
[ ![Download Android-geth](https://api.bintray.com/packages/sqli-nantes/ethereum-android/android-geth/images/download.svg) ](https://bintray.com/sqli-nantes/ethereum-android/android-geth/_latestVersion)
[ ![Download Ethereum-android](https://api.bintray.com/packages/sqli-nantes/ethereum-android/ethereum-android/images/download.svg) ](https://bintray.com/sqli-nantes/ethereum-android/ethereum-android/_latestVersion)
[ ![Download Ethereum-java-core](https://api.bintray.com/packages/sqli-nantes/ethereum-android/ethereum-java-core/images/download.svg) ](https://bintray.com/sqli-nantes/ethereum-android/ethereum-java-core/_latestVersion)

## Why using Ethereum-Android
Ethereum-Android permit to handle blockchain transactions with an android device. Specificly, the android's applications would be able to read nodes' informations, managed personnal ethereum account, call functions contains in *smart-contract* or send transactions.

It is based on Web3.js, permitting IRC communication, and use Rx 1.x. In view to manipulate blockchain transaction, it permit to launch and drive a geth's node.

To use this package, you need to use Android 21 at least and Geth 1.4.

## Installation

## Getting Started
### Start an inproc node

Create your Application subclass of EthereumApplication
```java
import com.sqli.blockchain.android_geth.EthereumApplication;

public class MyApplication extends EthereumApplication{
    [...]
}
```

In each activity you can be notified when Geth service is available, registering to the event in onCreate method.
When onEthereumServiceReady is called, you can do everything you want, like update UI, or create instanciate EthereumJava (see next section).
```java
import com.sqli.blockchain.android_geth.EthereumService;

public class MainActivity implements EthereumService.EthereumServiceInterface{

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        [...]
        MyApplication application = (MyApplication) getApplication();
        application.registerGethReady(this);
    }

    @Override
    public void onEthereumServiceReady() {
        //Update UI to start comunication
        super.onEthereumServiceReady();
    }
}
```

### Connect to a node

In your Application class for example :
```java
[...]
public EthereumJava ethereumjava;
[...]

public EthereumJava getEthereumJava() {
    return ethereumjava;
}

@Override
public void onEthereumServiceReady() {
    ethereumjava = new EthereumJava.Builder()
            .provider(new AndroidIpcProvider(ethereumService.getIpcFilePath();))
            .build();

    super.onEthereumServiceReady();
}  
```

### Create a account

```java
String password = "passwd";
String accountId = ethereumJava.personal.createAccount(password);
boolean unlocked = ethereumJava.personal.unlockAccount(accountId);
```

### Call a node module
Example : get NodeInfo asynchronously 
```java
EthereumJava ethereumJava = ((MyApplication) getApplication()).getEthereumJava()
Observable<NodeInfo> observable = ethereumJava.admin.getNodeInfo()
observable.subscribe(new Action1<NodeInfo>() {
    @Override
    public void call(NodeInfo nodeInfo) {
        System.out.println(nodeInfo);  
    }
});

```


### Send a transaction

//TODO

### *smart-contract* instanciation

```
contract ContractExample {

    void foo(){
    }
}    
```

```java
interface ....{
//TODO
}

```

//TODO
```java
eth....
```

### Interact with deployed *smart-contract*

Call an function on smartcontract (asynchrone call)

```java
String accountId = "0x000000000000..."; /* put here the account which call the function */
contract.increase().sendTransactionAndGetMined(accountId,new BigInteger("90000"))
    .observeOn(AndroidSchedulers.mainThread())
    .subscribe(new Action1<Transaction>() {
        @Override
        public void call(Transaction s) {
            /*
                Call has been made on the contract
                can verify with the transaction reference
            */
        }
    });
```



### Listen to *smart-contract* events
Subscribe on event and be notice each 5 transactions. 

```java
contract.fivesMore.watch()
    .observeOn(AndroidSchedulers.mainThread())
    .subscribe(new Action1<SInt256>() {
        @Override
        public void call(SInt256 s) {
            /*
                you're notified that counter has been incremented 5 more times
                parameter 's' has the current counter's value.
                You can directly update your app view
            */
        }
    });
```

## Project Architecture 

//TODO
## Our architecture vs the world
//TODO
## Contribute
//TODO
## Licence

```
The MIT License (MIT)

Copyright (c) 2015 Damien Lecan

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
```


