# SqliteManager

**get sample apk from [Google Play Store][googlePlayStoreLink]**

<img src="https://raw.githubusercontent.com/Ashok-Varma/SqliteManager/master/sqlite_320_compressed.gif" width="300" height="550" />

## What is this library about?

Sqlite Manager helps to manage your android Sqlite Database very effectively with ease

**(currently under active development, expect to see new releases almost daily)**

## Features

* Add, Update and Delete entries from tables in UI (no sql).
* Check all the table entries in your database with preferred sort order
* Do custom queries on your database and results are displayed in UI 

## Download

Based on your IDE you can import library in one of the following ways

Gradle:
```groovy
debugCompile 'com.ashokvarma.android:sqlite-manager:1.2.1'
releaseCompile 'com.ashokvarma.android:sqlite-manager-no-op:1.2.1'
```
If you want this in library in production also then try this : 
```groovy
compile 'com.ashokvarma.android:sqlite-manager:1.2.1'
```


or grab via Maven:
```xml
<dependency>
  <groupId>com.ashokvarma.android</groupId>
  <artifactId>sqlite-manager</artifactId>
  <version>1.2.1</version>
  <type>pom</type>
</dependency>
```

or Ivy:
```xml
<dependency org='com.ashokvarma.android' name='sqlite-manager' rev='1.2.1'>
  <artifact name='$AID' ext='pom'></artifact>
</dependency>
```

or Download [the latest JAR][mavenAarDownload]


## Usage

Implement SqliteDataRetriever interface either directly in SqliteOpenHelper (OR) if you are using thrid party ORM's ..etc just provide the inteface to library

Here is an example interface if SqliteOpenHelper is used (for other library implementations - check sample project)
```java
    public class HelperSqliteDataRetriever implements SqliteDataRetriever {
        SqliteHelper mSqliteHelper;
        SQLiteDatabase mSQLiteDatabase;

        HelperSqliteDataRetriever(SqliteHelper sqliteHelper) {
            mSqliteHelper = sqliteHelper;
            mSQLiteDatabase = mSqliteHelper.getWritableDatabase();
        }

        @Override
        public Cursor rawQuery(@NonNull String query, String[] selectionArgs) {
            if (mSQLiteDatabase == null || !mSQLiteDatabase.isOpen()) {
                mSQLiteDatabase = mSqliteHelper.getWritableDatabase();
            }
            return mSQLiteDatabase.rawQuery(query, selectionArgs);
        }

        @Override
        public String getDatabaseName() {
            return mSqliteHelper.getDatabaseName();
        }

        @Override
        public void freeResources() {
            // not good practice to open multiple database connections and close every time
        }
    }
```

Then just pass the interface instance with the context to launchSqliteManager method

```java
    SqliteManager.launchSqliteManager(this, new HelperSqliteDataRetriever(sqliteHelper), null);
```

### CSV/Json Export feature 
To Use export as CSV/Json feature. need to define FileProvider in your app manifest.
#### 1. If you don't have file-provider in your manifest
1. Add this section under application tag in your manifest
```xml
<provider
    android:name="android.support.v4.content.FileProvider"
    android:authorities="${applicationId}"
    android:exported="false"
    android:grantUriPermissions="true">

    <meta-data
        android:name="android.support.FILE_PROVIDER_PATHS"
        android:resource="@xml/file_provider_paths" />
</provider>
```
2. create a new xml file with name file_provider_paths and paste the below content in the file
```xml
<?xml version="1.0" encoding="utf-8"?>
<paths>
    <files-path
        name="sqlite_dump"
        path="sqliteManager/" />
</paths>
```
3. while launching sqlite manager pass application_id
```java
    SqliteManager.launchSqliteManager(this, new HelperSqliteDataRetriever(sqliteHelper), BuildConfig.APPLICATION_ID);
```
#### 2. If you already have a file-provider in your manifest
1. add files-path mentioned below to existing paths in your xml file.
```xml
<?xml version="1.0" encoding="utf-8"?>
<paths>
    <!-- -.-.-.-.-.-.-.-.-.-.- your current paths, caches ..etc -.-.-.-.-.-.-.-.-.-.- -->
    <files-path
        name="sqlite_dump"
        path="sqliteManager/" />
</paths>
```
2. while launching sqlite manager pass the authorities declared in your manifest
```java
    SqliteManager.launchSqliteManager(this, new HelperSqliteDataRetriever(sqliteHelper), "authority_string_mentioned_in_your_manifest");
```

## License

```
Sqlite Manager library for Android
Copyright (c) 2016 Ashok Varma (http://ashokvarma.me/).

Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.
```

## Other Open Source Libraries
1. [Gander](https://github.com/Ashok-Varma/Gander) : Gander is a simple in-app HTTP inspector for Android OkHttp clients. Gander intercepts and persists all HTTP requests and responses inside your application, and provides a UI for inspecting their content.
2. [SharedPrefManager](https://github.com/Ashok-Varma/SharedPrefManager) : SharedPref Manager is a Dev Debug tool that helps to manage(Edit, Add, Clear) your android Shared Preferences. 
3. [BottomNavigation](https://github.com/Ashok-Varma/BottomNavigation) : This Library helps users to use Bottom Navigation Bar (A new pattern from google) with ease and allows ton of customizations.

[mavenAarDownload]: https://repo1.maven.org/maven2/com/ashokvarma/android/sqlite-manager/1.2.1/sqlite-manager-1.2.1.aar
[googlePlayStoreLink]: https://play.google.com/store/apps/details?id=com.ashokvarma.sqlitemanager.sample
