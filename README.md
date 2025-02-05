# recyclerviewUnsplash merge two projects and use Sqlite for Android:

[![License](https://img.shields.io/github/license/unsplash/unsplash-photopicker-android.svg?style=flat-square)](https://github.com/unsplash/unsplash-photopicker-android)
[![Download](https://api.bintray.com/packages/unsplash/unsplash-photopicker-android/com.unsplash.pickerandroid.photopicker/images/download.svg) ](https://bintray.com/unsplash/unsplash-photopicker-android/com.unsplash.pickerandroid.photopicker/_latestVersion)

recyclerviewUnsplash merge two projects and use Sqlite for Android:

1. UnsplashPhotoPicker is an Android UI component that allows you to quickly search the Unsplash library for free high-quality photos with just a few lines of code.

2. PhotoPicker is and Android UI to allow view images download from Unplash and save under sqlite to work offline.

You can DOWLOAD "pruebatecnia.apk" [here](https://github.com/ezsocial/recyclerviewUnsplash/blob/main/pruebatecnica.apk) to Show how works our test, save images locally to work offline, when select single or multiples images.

[![Download](https://api.bintray.com/packages/unsplash/unsplash-photopicker-android/com.unsplash.pickerandroid.photopicker/images/download.svg) ](https://github.com/ezsocial/recyclerviewUnsplash/blob/main/pruebatecnica.apk)

![PhotoPicker and Photo Picker recyclerView for Android](https://github.com/ezsocial/recyclerviewUnsplash/blob/main/screens/Screen00.jpg "PhotoPicker for Android")

## Table of Contents

- [Description](#description)
- [Requirements](#requirements)
- [Installation](#installation)
  - [Gradle](#gradle)
- [Usage](#usage)
  - [Configuration](#configuration)
  - [Presenting](#presenting)
  - [Using the results](#using-the-results)
- [License](#license)

## Description

`UnsplashPhotoPicker` is a Kotlin object you use to initialize the library. You present the picker by navigating to the `UnsplashPickerActivity` to offer your users to select one or multiple photos from Unsplash. Once they have selected photos, the `UnsplashPickerActivity` returns `UnsplashPhoto` objects that you can use in your app.

## Requirements

- Android minimum API 21+
- Android Studio 3.3+
- Kotlin 1.3+
- Use AndroidX artifacts when creating your project
- [Unsplash API Access Key and Secret Key](https://unsplash.com/documentation#registering-your-application)

## Installation

### Gradle

To integrate `UnsplashPhotoPicker` into your Android Studio project using Gradle, specify in your project `build.gradle` file:

```gradle
allprojects {
   repositories {
      maven { url  "https://dl.bintray.com/unsplash/unsplash-photopicker-android" }
   }
}
```
And in your app module `build.gradle` file:

```gradle
dependencies {
   implementation 'com.unsplash.pickerandroid:photopicker:x.y.z'
}
```

## Usage

❗️Before you get started, you need to register as a developer on our [Developer](https://unsplash.com/developers) portal. Once registered, create a new app to get an **Access Key** and a **Secret Key**.

### Configuration

The `UnsplashPhotoPicker` is a Kotlin object you need to use in order to initialize the library. Add this in your custom application class `onCreate` method:

```kotlin
UnsplashPhotoPicker.init(
            this, // application
            "your access key",
            "your secret key"
            /* optional page size */
        )
```
| Property                      | Type          | Optional/Required | Default |
|-------------------------------|---------------|-------------------|---------|
| **`application`**             | _Application_ | Required          | N/A     |
| **`accessKey`**               | _String_      | Required          | N/A     |
| **`secretKey`**               | _String_      | Required          | N/A     |
| **`pageSize`**                | _Int_         | Optional          | `20`    |

### Presenting

In order to access the picker screen you need to navigate to the `UnsplashPickerActivity`. This activity has a `getStartingIntent` method to create the `Intent` needed:

```kotlin
startActivityForResult(
                UnsplashPickerActivity.getStartingIntent(
                    this, // context
                    isMultipleSelection
                ), REQUEST_CODE
            )
```

### Using the results by this HappyMonday test *Unsplash and PhotoView*

Your calling activity must use a `startActivityForResult` method to be able to retrieve the selected `UnsplashPhoto`:

```kotlin
override fun onActivityResult(requestCode: Int, resultCode: Int, data: Intent?) {
        super.onActivityResult(requestCode, resultCode, data)
        if (resultCode == Activity.RESULT_OK && requestCode == REQUEST_CODE) {
            val photos: ArrayList<UnsplashPhoto>? = data?.getParcelableArrayListExtra(UnsplashPickerActivity.EXTRA_PHOTOS)
            // use your photos here
        }
}


```

Your calling recycler_view must use a `mAdapter` method to be able to retrieve the selected `UnsplashPhoto`:


``` java
main_recycler_view.adapter = mAdapter
        // on the pick button click, we start the library picker activity
        // we are expecting a result from it so we start it for result
        main_pick_button.setOnClickListener {
            startActivityForResult(
                    UnsplashPickerActivity.getStartingIntent(
                            this,
                            !main_single_radio_button.isChecked
                    ), REQUEST_CODE
            )
        }
```

### Create sqlite database and use 

Your calling Sqlitet library to be able to save locallye and works offline the selected `images`:

```java
public class DBHelper extends SQLiteOpenHelper {

    private static final String DB_NAME = "happymondays.db2";
    private static final int DB_SCHEME_VERSION = 2;

    public DBHelper(Context context) {
        super(context, DB_NAME, null, DB_SCHEME_VERSION);
    }

    @Override
    public void onCreate(SQLiteDatabase db) {

        db.execSQL(DataBaseManager.CREATE_GALLERY);
        db.execSQL(DataBaseManager.CREATE_LOGIN);
    }

    @Override
    public void onUpgrade(SQLiteDatabase db, int oldVersion, int newVersion) {
        db.execSQL("DROP TABLE IF EXISTS gallery");
        db.execSQL("DROP TABLE IF EXISTS login");
        onCreate(db);
    }
}

```
See [UnsplashPhoto.kt](https://github.com/unsplash/unsplash-photopicker-android/blob/master/photopicker/src/main/java/com/unsplash/pickerandroid/photopicker/data/UnsplashPhoto.kt) for more details.

## License

MIT License

Copyright (c) 2019 Unsplash Inc.

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the "Software"), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.

