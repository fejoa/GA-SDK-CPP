GA-SDK-CPP
==========

GameAnalytics C++ SDK

Supported platforms:

* Mac OS X
* Winddows 32-bit and 64-bit
* Linux
* UWP
* Tizen

Dependencies
------------

* python 2.7 or higher (not working for python 3)
  * **Windows:** Go [here](https://www.python.org/downloads/) to download and install Python
  * **Mac:** Should come with Python out of the box or else run *'brew install python'*
* cmake (will be automatically downloaded)
* **Mac:** XCode
* **Windows:** Visual Studio

Changelog
---------
**1.3.6**
* bug fix for end session when using manual session handling

**1.3.5**
* session length precision improvement

**1.3.4**
* custom user id bug fix

**1.3.3**
* bug fix to dupplicate logs send to console (windows, mac, linux)

**1.3.2**
* added OS version, device model and device manufacturer to event (windows, mac)

**1.3.1**
* added support for Linux
* logging initialisation updated for windows and mac (windows, mac)

**1.3.0**
* added support for Tizen

**1.2.4**
* removed unused files

**1.2.3**
* fixed build script for UWP
* changed persistent path for UWP

**1.2.2**
* fixed issue with onstop method

**1.2.1**
* possible to set custom dimensions and demographics before initialise

**1.2.0**
* added UWP support

**1.1.1**
* fix to empty user id

**1.1.0**
* switched to use curl as network library

**1.0.1**
* fix for empty user id in events

**1.0.0**
* Initial version with Windows and Mac OS X support

How to build
------------

To start a build for all supported targets simply call

**Mac: ./build.sh**

**Windows: build.bat** (must be run as administrator when building for Tizen)

Or call **build.bat -h** or **./build.sh -h** to display the usage of the script.

How to develop
--------------

If you developing the SDK, you can use the generated project files to work with.

They are located under **build/jenkins/build/[target]** after calling the **build.sh** or **build.bat**.


Folderstructure
---------------

* **build** - Contains scripts for automated project setup, building and creating of the final exstracted library
* **build/cmake** - Contains the cmake files for the project
* **build/jenkins** - Contains the scripts and working directories for the automated builds
* **export** - Target folder for the automated export of the GA lib
* **source** - Contains the complete source code for the project including the dependencies
* **tests** - Contains tests for testing the functionality in the GA SDK, to run tests run **tests/run_tests_osx.py** (mac only)

Lib Dependencies
----------------

* **crossguid** (*as source*) - Cross platform library to generate a Guid.
* **cryptoC++** (*as source*) - collection of functions and classes for cryptography related tasks.
* **curl** (*as binary*) - library used to make HTTP requests.
* **jsonCpp** (*as source*) - lightweight C++ library for manipulating JSON values including serialization and deserialization.
* **openssl** (*as binary*) - used by **curl** to make HTTPS requests.
* **plog** (*as source*) - library used for logging.
* **SQLite** (*as source*) - SQLite is a software library that implements a self-contained, serverless, zero-configuration, transactional SQL database engine.

*as source* means the dependency will be compiled with the project itself, *as binary* means the dependency is prebuild and will be linked to the project

Preperation before usage of the SDK
-----------------------------------

* Include the following directories in your C++ project's include paths:
  * **source/gameanalytics**

* In your C++ project, link to the following libraries:
  * All libraries in **source/dependencies/curl/lib/[target]**
  * All libraries in **source/dependencies/openssl/1.0.2h/libs/[target]**
  * Produced library from build script found in **export/[target]-static/Release/**


Usage of the SDK
----------------

Remember to include the GameAnalytics header file wherever you are using the SDK:

``` c++
 #include "GameAnalytics.h"
```

###Configuration

Example:

``` c++
 gameanalytics::GameAnalytics::setEnabledInfoLog(true);
 gameanalytics::GameAnalytics::setEnabledVerboseLog(true);

 gameanalytics::GameAnalytics::configureBuild("0.10");

 {
     std::vector<std::string> list;
     list.push_back("gems");
     list.push_back("gold");
     gameanalytics::GameAnalytics::configureAvailableResourceCurrencies(list);
 }
 {
     std::vector<std::string> list;
     list.push_back("boost");
     list.push_back("lives");
     gameanalytics::GameAnalytics::configureAvailableResourceItemTypes(list);
 }
 {
     std::vector<std::string> list;
     list.push_back("ninja");
     list.push_back("samurai");
     gameanalytics::GameAnalytics::configureAvailableCustomDimensions01(list);
 }
 {
     std::vector<std::string> list;
     list.push_back("whale");
     list.push_back("dolphin");
     gameanalytics::GameAnalytics::configureAvailableCustomDimensions02(list);
 }
 {
     std::vector<std::string> list;
     list.push_back("horde");
     list.push_back("alliance");
     gameanalytics::GameAnalytics::configureAvailableCustomDimensions03(list);
 }
```

###Initialization

Example:

``` c++
 gameanalytics::GameAnalytics::initialize("<your game key>", "<your secret key");
```

###Send events

Example:

``` c++
 gameanalytics::GameAnalytics::addDesignEvent("testEvent");
 gameanalytics::GameAnalytics::addBusinessEvent("USD", 100, "boost", "super_boost", "shop");
 gameanalytics::GameAnalytics::addResourceEvent(gameanalytics::Source, "gems", 10, "lives", "extra_life");
 gameanalytics::GameAnalytics::addProgressionEvent(gameanalytics::Start, "progression01", "progression02");
```
