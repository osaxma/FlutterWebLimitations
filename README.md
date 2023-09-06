# Flutter Web Limitations
a list of Flutter web limitations.




## DateTime precision is limited to milliseconds:

Be careful when relying on microseconds precision, according to the Dart team:
> The web Date object doesn't support microseconds. It's implemented using the JavaScript Date object which only supports millisecond precision. So, working as well as possible.


```dart 
main() {
 final datetimeParsed = DateTime.parse('2021-02-05T22:37:12.933232+00:00');
 final datetimeMS = DateTime.fromMicrosecondsSinceEpoch(1612564632933232);
  
 print(datetimeParsed.toIso8601String()); // prints 2021-02-05T22:37:12.933Z
 print(datetimeMS.toUtc().toIso8601String()); // prints 2021-02-05T22:37:12.933Z
  
 print(datetimeParsed.microsecondsSinceEpoch); // prints 1612564632933000
 print(datetimeMS.microsecondsSinceEpoch); // prints 1612564632933000 
}
```
see [dartlang/issues/44876](https://github.com/dart-lang/sdk/issues/44876)

## Dart SDK libraries not available in Flutter:
- `dart:io` which includes `Platform` to detect current platform and `File` for transferring bytes. 
<!--  add alternative solutions  -->

see [flutter/flutter/39998](https://github.com/flutter/flutter/issues/39998)

## Flutter Platform Detection

In Flutter web, you cannot use `Platform` from `dart:io` (e.g. `Platform.iOS` or `Platform.Android`). In order to detect the platform, you need to import the `foundation` package such as:

```dart
import 'package:flutter/foundation.dart';

bool get isOS => defaultTargetPlatform == TargetPlatform.iOS;
bool get isAndroid => defaultTargetPlatform == TargetPlatform.android;
bool get isMacOS => defaultTargetPlatform == TargetPlatform.macOS;
bool get isWindows => defaultTargetPlatform == TargetPlatform.windows;
bool get isLinux => defaultTargetPlatform == TargetPlatform.linux;

// Extra: A handy way to detect to know if the client is on Web using Mobile:
bool get isWebMobile => kIsWeb &&
    (defaultTargetPlatform == TargetPlatform.iOS ||
        defaultTargetPlatform == TargetPlatform.android);
```

In addition, if you like to detect if 


## Flutter Web is not SEO friendly

Flutter Web is not SEO friendly [see flutter/issues/46789](https://github.com/flutter/flutter/issues/46789)
making it hard, if even possible, to motetize with ADS [see flutter/issues/40376](https://github.com/flutter/flutter/issues/40376)

## Flutter Web JsonEncode behavior	
In Web, using `jsonEncode` will convert `double` values to `int` when the fractional part is equal to zero as shown below:	

```dart	
import 'dart:convert';	
void main() {	
  final double x = 3.0;	
  final double y = 3.1;	
  final jsonString = jsonEncode({'x': x, 'y': y});	
  	
  print(jsonString); // prints {"x":3,"y":3.1}	
   	
  // Decoding jsonString above works fine in web. 	
  // Though decoding jsonString in iOS/Android will throw an error	
  // when the expected value is double	
  // e.g.	
  //    `DataClass.fromJson(jsonString);` 	
  //    where DataClass has x & y defined as doubles 	
}	
```

For more details, [see dart-lang/issues/45160](https://github.com/dart-lang/sdk/issues/45160)

## Flutter Web does not support "Hot Reload"

Flutter web supports `Hot Restart`, but not `Hot Reload`, see issue [flutter/issues/53041](https://github.com/flutter/flutter/issues/53041)

## Flutter Web does not support "dart-grpc"

Flutter Web does not support and seemingly doesn't plan to support [dart-grpc](https://pub.dev/packages/grpc) [flutter/issues/48054](https://github.com/flutter/flutter/issues/48054#issuecomment-571254564)
this package doesn't figure in the [List of dart sdk libraries not available in Flutter Web](flutter/flutter/39998)

## GIFs have some issues in PWA (mobile):
GIFs in mobile browsers for Flutter Web have some ongoing issues (e.g., not playing at all or stops playing on scrolling and never play again, etc). see [flutter/issues/78223](https://github.com/flutter/flutter/issues/78223)
