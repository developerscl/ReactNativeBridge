A bridge to React Native
========================

TagCommander's SDK being build for native platforms, it is not available to use in React Native. But like all the non-native technologies, React has a way to make a connection between native libraries and the language it is using. This is called a Bridge and this document is there to explain how it is done and used.


The following links served as a base for this project: 

* http://facebook.github.io/react-native/docs/native-modules-android.html
* https://shift.infinite.red/native-modules-for-react-native-android-ac05dbda800d
* http://facebook.github.io/react-native/docs/native-modules-ios.html
* http://facebook.github.io/react-native/docs/linking-libraries-ios.html

The samples will be separated for Android and iOS.

We implemented the three most important methods of the SDK here. The initialize method, a method to add data and the one to send all those data. It should covers around 90% of client's usages. If you need more, please contact the person in charge of your project at Tag Commander.

Android
=======

Important files
---------------

in /android/app/src/main/java/com/tcwithreact/ please check:

* TCReactPackage.java   
* TCWrapper.java

Steps
-----

* Copy TagCommander's Jar inside your android project.
* Create a ReactPackage, a wrapper object grouping many modules (both native and JavaScript) together, and include it in the getPackages method of MainActivity.
* Create a Java class extending ReactContextBaseJavaModule that implements the desired functionality and register it with our ReactPackage.
* Override the getName method in the aforementioned class. This will be the name of the native module in JavaScript.
* Expose desired public methods to JavaScript by annotating them with @ReactMethod.
* Finally, import the module from NativeModules in your JavaScript code and call the methods.

Copy the Jar
------------

You first need to create a libs folder : android/app/libs
Then take the Jar from TagCommander and copy it inside.

It is possible that gradle doesn't compile automitically with the jar for some reasons. If there is any issue, open build.gradle and add the following line inside the dependencies.

	compile files('libs/TagCommander.jar')

ReactPackage
------------

You might already use one, but since this project only use TagCommander we created one for this purpose only. You can find the file TCReactPackage.java associated with this documentation.

Here, it's only used to link our module to the NativeModules.

ReactContextBaseJavaModule
--------------------------

Here is the main part about the bridging process. We have a class that call directly the native SDK and you can make "React visible" the methods you implement there.

This class is called TCWrapper.java, you can check it for a more detailed view of the bridge.

MainApplication
---------------

In Android you need a final step which is instanciating the class you use for the bridge in getPackages().

In our sample, here is what we have done :

    @Override
    protected List<ReactPackage> getPackages()
    {
      return Arrays.<ReactPackage>asList(
          new MainReactPackage(),
          // We add our package to the list
          new TCReactPackage()
      );
    }

iOS
===

**For react-native version above 0.4 please use release 1.1.0.**

Important files
---------------

in /ios/ please check:

* TCWrapper.h   
* TCWrapper.m


Steps
-----

* Add the library to your project
* Add TCWrapper to your project
* Done


Add Library and Wrapper
-----------------------

Open XCode and simply copy the directory "include" into the "Libraries" folder, you can use the option "Copy if needed". XCode will do everything else. Do the same for libTagCommander.a.

Do the same the TCWrapper.h and TCWrapper.m in your project sources and it's done.

Take a look at the given ios project to see the setup.

Calling the bridges
===================

From this point all that is left is calling the code.

First add the required imports :

	import { NativeModules } from 'react-native';
	module.exports = NativeModules.TCWrapper;

Then simply call the methods you want to use :

	NativeModules.TCWrapper.initTagCommander(1263, 39);

