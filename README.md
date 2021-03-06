![Timber](logo.png)

This is a logger with a small, extensible API which provides utility on top of Android's normal
`Log` class.

I copy this class into all the little apps I make. I'm tired of doing it. Now it's a library.

Behavior is added through `Tree` instances. You can install an instance by calling `Timber.plant`.
Installation of `Tree`s should be done as early as possible. The `onCreate` of your application is
the most logical choice.

The `DebugTree` implementation will automatically figure out from which class it's being called and
use that class name as its tag. Since the tags vary, it works really well when coupled with a log
reader like [Pidcat][1].

There are no `Tree` implementations installed by default because every time you log in production, a
puppy dies.


Usage
-----

Two easy steps:

 1. Install any `Tree` instances you want in the `onCreate` of your application class.
 2. Call `Timber`'s static methods everywhere throughout your app.

Check out the sample app in `timber-sample/` to see it in action.

Replace all Log calls (Android Studio/IntelliJ)
-----------------------------------------------
In Android Studio, you can make a structural search as outlined in the [Android Dev Summit video](https://youtu.be/Y2GC6P5hPeA?t=6m29s). 
 1. Find the action  _"Replace Structurally"_ by `Shift+Cmd+A` (`Ctrl+Shift+A` on Windows/Linux).
 2. Search template: `android.util.Log.$log_level$($tag$, $message$)`
 3. Replace template: `timber.log.Timber.$log_level$($message$)`
 4. Define the scope (whole project, module, etc.)

Lint
----

Timber ships with embedded lint rules to detect problems in your app.

 *  **TimberArgCount** (Error) - Detects an incorrect number of arguments passed to a `Timber` call for
    the specified format string.

        Example.java:35: Error: Wrong argument count, format string Hello %s %s! requires 2 but format call supplies 1 [TimberArgCount]
            Timber.d("Hello %s %s!", firstName);
            ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

 *  **TimberArgTypes** (Error) - Detects arguments which are of the wrong type for the specified format string.

        Example.java:35: Error: Wrong argument type for formatting argument '#0' in success = %b: conversion is 'b', received String (argument #2 in method call) [TimberArgTypes]
            Timber.d("success = %b", taskName);
                                     ~~~~~~~~
 *  **TimberTagLength** (Error) - Detects the use of tags which are longer than Android's maximum length of 23.

        Example.java:35: Error: The logging tag can be at most 23 characters, was 35 (TagNameThatIsReallyReallyReallyLong) [TimberTagLength]
            Timber.tag("TagNameThatIsReallyReallyReallyLong").d("Hello %s %s!", firstName, lastName);
            ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

 *  **LogNotTimber** (Warning) - Detects usages of Android's `Log` that should be using `Timber`.

        Example.java:35: Warning: Using 'Log' instead of 'Timber' [LogNotTimber]
            Log.d("Greeting", "Hello " + firstName + " " + lastName + "!");
                ~

 *  **StringFormatInTimber** (Warning) - Detects `String.format` used inside of a `Timber` call. Timber
    handles string formatting automatically.

        Example.java:35: Warning: Using 'String#format' inside of 'Timber' [StringFormatInTimber]
            Timber.d(String.format("Hello, %s %s", firstName, lastName));
                     ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

 *  **BinaryOperationInTimber** (Warning) - Detects string concatenation inside of a `Timber` call. Timber
    handles string formatting automatically and should be preferred over manual concatenation.

        Example.java:35: Warning: Replace String concatenation with Timber's string formatting [BinaryOperationInTimber]
            Timber.d("Hello " + firstName + " " + lastName + "!");
                     ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~


Download
--------

```groovy
compile 'com.jakewharton.timber:timber:4.1.0'
```

Snapshots of the development version are available in [Sonatype's `snapshots` repository][snap].


License
-------

    Copyright 2013 Jake Wharton

    Licensed under the Apache License, Version 2.0 (the "License");
    you may not use this file except in compliance with the License.
    You may obtain a copy of the License at

       http://www.apache.org/licenses/LICENSE-2.0

    Unless required by applicable law or agreed to in writing, software
    distributed under the License is distributed on an "AS IS" BASIS,
    WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
    See the License for the specific language governing permissions and
    limitations under the License.



 [1]: http://github.com/JakeWharton/pidcat/
 [2]: https://search.maven.org/remote_content?g=com.jakewharton.timber&a=timber&v=LATEST
 [3]: http://square.github.io/dagger/
 [4]: http://jakewharton.github.io/butterknife/
 [snap]: https://oss.sonatype.org/content/repositories/snapshots/
