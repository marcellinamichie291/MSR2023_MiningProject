Annoucement: There's a new OneDrive SDK
================
The Live SDK has been replaced by the OneDrive API and [OneDrive SDK for Android](https://github.com/OneDrive/onedrive-sdk-android). All new projects should use the OneDrive API to integrate with OneDrive instead of Live SDK.

This Live SDK project will remain available for existing applications to continue to use but new application development should be done using the OneDrive SDK.

Live SDK for Android (Legacy)
====================

version 5.6

1. Introduction
---------------

The Live SDK for Android library is intended to help developers to easily 
integrate OneDrive and Outlook.com contacts into their Android apps.

For questions on how to use the sdk visit http://stackoverflow.com/questions/tagged/onedrive

2. Reference the API
--------------------

After you download the Android API source code, you must compile it. You need
to do this only once. After you compile the Android API source code on your
computer, you can reference it from multiple Eclipse Android projects on that
same computer.

To compile the Android API source code  
* Start Eclipse, if it is not already running.
* Click File > Import.
* Expand General, click Existing Projects into Workspace, and then click Next.
* With the Select root directory option selected, click Browse.
* Go to and select the src folder within the downloaded Live SDK for Android, and then click OK.
* In the Projects box, select the LiveSdk check box.
* With the Copy projects into workspace check box selected, click Finish.
  Eclipse adds the LiveSdk project to the Package Explorer pane and then
  compiles the Android API source code in the background.
* You can now reference the compiled Android API source code from your Eclipse
  Android projects.

  
To reference the compiled Android API source code in an Eclipse Android
project
* In Eclipse, display the Package Explorer pane, if it is not already visible.
* Right-click your project's name, and then click Properties.
* In the list of project properties, click Android.
* In the Library area, click Add.
* Click LiveSdk and then click OK.
* Click OK. Eclipse sets a reference to the compiled Android API source code
  project, and you can now call the Live Connect Android API from your own
  Android project.

Note: Before you run your project, you must add the Internet permission to your
      project's manifest, as shown in the following steps. If you don't add the
      Internet permission, your app may have problems accessing Live Connect
      web services later.
* With your project open, in the Package Explorer pane, open the AndroidManifest.xml file.
* In the editor, click the Permissions tab.
* Click Add.
* Click Uses Permission, and then click OK.
* the Name list, click android.permission.INTERNET.
* Save the AndroidManifest.xml file.

Notes:
The Java Compiler must be version 1.6 or higher.

3. Documentation
----------------

Visit: http://dev.onedrive.com and click the "Documentation" link. 

4. Sample project
-----------------

There is a sample project located in the sample directory. Using eclipse this project can be added
using the File > Import > General > Existing Projects into Workspace wizard. Before the project can be
properly used, the static variable com.microsoft.live.sample.Config.CLIENT_ID (see Config.java) must
be changed to your own client ID. To obtain a Client ID, visit the [Live SDK App management page] (http://go.microsoft.com/fwlink/p/?LinkId=193157)

5. Known issues:
----------------

1) Login dialog is destroyed on screen rotation if the Activity is not set to
   ignore orientation changes in AndroidManifest.xml.


License Agreement
-----------------

Copyright (c) 2014 Microsoft Corporation

Permission is hereby granted, free of charge, to any person obtaining a copy
 of this software and associated documentation files (the "Software"), to deal
 in the Software without restriction, including without limitation the rights
 to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
 copies of the Software, and to permit persons to whom the Software is
 furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in
 all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
 IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
 FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
 AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
 LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
 OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN
 THE SOFTWARE.
