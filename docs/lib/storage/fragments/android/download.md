### Download to file

If you uploaded the data using the key `ExampleKey`, you can retrieve the data using `Amplify.Storage.downloadFile`.

<amplify-block-switcher>
<amplify-block name="Java">

```java
Amplify.Storage.downloadFile(
        "ExampleKey",
        new File(getApplicationContext().getFilesDir() + "/download.txt"),
        result -> Log.i("MyAmplifyApp", "Successfully downloaded: " + result.getFile().getName()),
        error -> Log.e("MyAmplifyApp",  "Download Failure", error)
);
```

</amplify-block>
<amplify-block name="Kotlin - Callbacks">

```kotlin
val file = File("${applicationContext.filesDir}/download.txt")
Amplify.Storage.downloadFile("ExampleKey", file,
    { Log.i("MyAmplifyApp", "Successfully downloaded: ${it.file.name}") },
    { Log.e("MyAmplifyApp",  "Download Failure", it) }
)
```

</amplify-block>
<amplify-block name="Kotlin - Coroutines (Beta)">

```kotlin
try {
    val file = File("${applicationContext.filesDir}/download.txt")
    val download = Amplify.Storage.downloadFile("ExampleKey", file)
    try {
        val fileName = download.result().file.name
        Log.i("MyAmplifyApp", "Successfully downloaded: $fileName")
    } catch (error: StorageException) {
        Log.e("MyAmplifyApp", "Download Failure", error)
    }
}
```

</amplify-block>
<amplify-block name="RxJava">

```java
RxProgressAwareSingleOperation<StorageDownloadFileResult> download =
        RxAmplify.Storage.downloadFile(
            "ExampleKey",
            new File(getApplicationContext().getFilesDir() + "/download.txt"
        );

download
    .observeResult()
    .subscribe(
        result -> Log.i("MyAmplifyApp", "Successfully downloaded: " + result.getFile().getName()),
        error -> Log.e("MyAmplifyApp",  "Download Failure", error)
    );
```

</amplify-block>
</amplify-block-switcher>

### Track download progress

To track progress of the download, use the `downloadFile` API that includes a progress listener callback.

<amplify-block-switcher>
<amplify-block name="Java">

```java
Amplify.Storage.downloadFile(
        "ExampleKey",
        new File(getApplicationContext().getFilesDir() + "/download.txt"),
        StorageDownloadFileOptions.defaultInstance(),
        progress -> Log.i("MyAmplifyApp", "Fraction completed: " + progress.getFractionCompleted()),
        result -> Log.i("MyAmplifyApp", "Successfully downloaded: " + result.getFile().getName()),
        error -> Log.e("MyAmplifyApp",  "Download Failure", error)
);
```

</amplify-block>
<amplify-block name="Kotlin - Callbacks">

```kotlin
val file = File("${applicationContext.filesDir}/download.txt")
val options = StorageDownloadFileOptions.defaultInstance()
Amplify.Storage.downloadFile("ExampleKey", file, options,
    { Log.i("MyAmplifyApp", "Fraction completed: ${it.fractionCompleted}") },
    { Log.i("MyAmplifyApp", "Successfully downloaded: ${it.file.name}") },
    { Log.e("MyAmplifyApp", "Download Failure", it) }
)
```

</amplify-block>
<amplify-block name="Kotlin - Coroutines (Beta)">

```kotlin
val file = File("${applicationContext.filesDir}/download.txt")
val options = StorageDownloadFileOptions.defaultInstance()
val download = Amplify.Storage.downloadFile("ExampleKey", file, options)
val progressJob = activityScope.async {
    download.progress().collect { progress ->
        Log.i("MyAmplifyApp", "Fraction completed: ${progress.fractionCompleted}")
    }
}
try {
    val fileName = download.result().file.name
    Log.i("MyAmplifyApp", "Successfully downloaded: $fileName")
} catch (error: StorageException) {
    Log.e("MyAmplifyApp", "Download Failure", error)
}
progressJob.cancel()
```

</amplify-block>
<amplify-block name="RxJava">

```java
RxProgressAwareSingleOperation<StorageDownloadFileResult> download =
        RxAmplify.Storage.downloadFile(remoteKey, localFile);

download
    .observeProgress()
    .subscribe(
      progress -> Log.i("MyAmplifyApp", progress.getFractionCompleted())
    );
```

</amplify-block>
</amplify-block-switcher>

### Generate a download URL

You can also retrieve a URL for the object in storage:

<amplify-block-switcher>
<amplify-block name="Java">

```java
Amplify.Storage.getUrl(
    "ExampleKey",
    result -> Log.i("MyAmplifyApp", "Successfully generated: " + result.getUrl()),
    error -> Log.e("MyAmplifyApp", "URL generation failure", error)
);
```

</amplify-block>
<amplify-block name="Kotlin - Callbacks">

```kotlin
Amplify.Storage.getUrl(
    "ExampleKey",
    { Log.i("MyAmplifyApp", "Successfully generated: ${it.url}") }
    { Log.e("MyAmplifyApp", "URL generation failure", it) }
)
```

</amplify-block>
<amplify-block name="Kotlin - Coroutines (Beta)">

```kotlin
try {
    val url = Amplify.Storage.getUrl("ExampleKey").url
    Log.i("MyAmplifyApp", "Successfully generated: $url")
} catch (error: StorageException) {
    Log.e("MyAmplifyApp", "URL generation failure", error)
}
```

</amplify-block>
<amplify-block name="RxJava">

```java
RxAmplify.Storage.getUrl("ExampleKey").subscribe(
    result -> Log.i("MyAmplifyApp", "Successfully generated: " + result.getUrl()),
    error -> Log.e("MyAmplifyApp", "URL generation failure", error)
);
```

</amplify-block>
</amplify-block-switcher>
