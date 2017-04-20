# source-monitor-complexity-attribution-issue

Reproduction details:
- Source Monitor v3.5.3.327
- Windows 7 Home Premium SP1 (x64)
- `ParseObject.java` test file taken from https://github.com/ParsePlatform/Parse-SDK-Android/blob/3441303006e0ae51c37a0008e4f891fb87afc2ee/Parse/src/main/java/com/parse/ParseObject.java

Steps:

1. Clone and run `run-source-monitor.bat` several times
2. Notice that reported _Most Complex Method_ is `deepSaveAsync().Task&lt;Void&gt;()` having `19` complexity on line `2280`, but this line [seems to point](https://github.com/parse-community/Parse-SDK-Android/blob/3441303006e0ae51c37a0008e4f891fb87afc2ee/Parse/src/main/java/com/parse/ParseObject.java#L2280) to unrelated method:
```java
public static <T extends ParseObject> void deleteAllInBackground(List<T> objects, DeleteCallback callback) {
	ParseTaskUtils.callbackOnMainThreadAsync(deleteAllInBackground(objects), callback);
}
```
3. Other complexities may also be unrelated besides `deepSaveAsync().Task&lt;Void&gt;()`. E.g. line `2831` is reported to have `?(instance of Continuation&lt;Void, Task&lt;Void&gt;&gt;)11.task.onSuccessTask()` method with complexity of `5`, but this line [points to](https://github.com/parse-community/Parse-SDK-Android/blob/3441303006e0ae51c37a0008e4f891fb87afc2ee/Parse/src/main/java/com/parse/ParseObject.java#L2831):
```java
public static <T extends ParseObject> void fetchAllInBackground(List<T> objects,
  FindCallback<T> callback) {
ParseTaskUtils.callbackOnMainThreadAsync(fetchAllInBackground(objects), callback);
}
```

See `sm-details-results.xml` and `sm-summary-results.xml` for sample results.
