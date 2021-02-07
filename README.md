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
