# Encrypted Core Data SQLite Store

Core Data encrypted SQLite store using [SQLCipher](http://sqlcipher.net). Use this security control to encrypt data stored in Core Data with SQLite by leveraging the great work at SQLCipher. With this control one no longer has to translate each query result in/out of data models.

# Vulnerabilities Addressed

1. SQLite database is not encrypted, contents are in plain text
  - CWE-311: Missing Encryption of Sensitive Data
2. SQLite database file protected with 4 digit system passcode
  - CWE-326: Inadequate Encryption Strength
  - SRG-APP-000129-MAPP-000029  Severity-CAT II: The mobile application must implement automated mechanisms to enforce access control restrictions which are not provided by the operating system

# Caveat
This library is a work in progress and will probably not work in every case or with highly complex models. There is also what I believe to be a bug in the implementation of `NSIncrementalStoreNode` and its use through the Core Data framework. I have an open DTS ticket with Apple and am working on this. The issue can be seen by changing the value of and searching for `USE_CUSTOM_NODE_CACHE` in `EncryptedStore.m`.

# Project Setup
  * When creating the project make sure **Use Core Data** is selected
  * Follow the [SQLCipher for iOS](http://sqlcipher.net/ios-tutorial/) setup guide
  * Switch into your project's root directory and checkout the encrypted-core-data project code
```
    cd ~/Documents/code/YourApp

    git clone https://github.com/project-imas/encrypted-core-data.git
```
  * Click on the top level Project item and add files ("option-command-a")
  * Navigate to **encrypted-core-data**, highlight **Incremental Store**, and click **Add**

# Using EncryptedStore

In your application delegate source file (i.e. AppDelegate.m) you should see
```objc
NSPersistentStoreCoordinator *coordinator = [self persistentStoreCoordinator];
```
replace that line with
```objc
NSPersistentStoreCoordinator *coordinator = [EncryptedStore makeStore:[self managedObjectModel]:@"SOME_PASSWORD"];
```

Replacing **SOME_PASSWORD** with an actual password.

Also in the same file add an import for EncryptedStore.h:
```objc
   #import "EncryptedStore.h"
```

# Debugging

`EncryptedStore` responds to the standard CoreData SQL debug flag. Add `-com.apple.CoreData.SQLDebug 1` as an argument to the "run" phase of your scheme to see all generated statements logged to the console.

# Improvements

This project has several areas that could be improved (in order of preference):

- **Migrations** The store currently supports a very small subset of lightweight migrations with inferred migration maps (changing columns, adding and removing whole objects). I would like to implement more migration map parsing.
- **Relationships** I currently only support one-to-many relationships. Work needs to be done in order to support many-to-many and one-to-one.
- **Inheritance** Table inheritance is not really supported or tested at this time.
- **More Test Cases** I built several test cases that helped find bugs and improve support for things like relationships. These cases currently do not touch every feature supported by the store.
- **Persistent Store Options** You have the option to pass a number of options when adding a new store a coordinator. I added my own option which provides the database key but would like to support system options as well. Things like data protection class, SQLite pragmas, and migration options would be nice to have.

# Resources

- [OpenSSL](http://www.openssl.org)
- [SQLCipher](http://sqlcipher.net)
- [SQLCipher in Xcode](http://sqlcipher.net/sqlcipher-binaries-ios-and-osx/)

## Use, Feedback, and Improvement

We strongly encourage developers to clone and use iMAS. Once you’ve had a chance to use iMAS, tell us what you think by providing us with feedback on your intended use. This information will enable us to address relevancy and need - which will help to keep our research funded in the long run. Lastly, feel free to enhance and improve the actual controls by submitting pull requests early and often!

## Recognition

MITRE wishes to thank [Caleb Davenport](https://github.com/calebmdavenport) for creating, implementing, and pushing for the public release of this security control.

## License

Copyright 2012 The MITRE Corporation, All Rights Reserved.

Licensed under the Apache License, Version 2.0 (the "License");
you may not use this work except in compliance with the License.
You may obtain a copy of the License at

    http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.

[![githalytics.com alpha](https://cruel-carlota.pagodabox.com/50cf88b71d3c78a0268ae42ea79d8951 "githalytics.com")](http://githalytics.com/project-imas/encrypted-core-data)


