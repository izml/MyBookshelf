# 版本升降级(minSdk=19)

## build文件

### 1. `build.gradle`

| 包                                               | 版本  | 修改为 | 备注                   |
| ------------------------------------------------ | ----- | ------ | ---------------------- |
| com.android.tools.build:gradle                   | 7.0.2 | 4.1.3  | 4.2.0+ minGradle=6.7.1 |
| com.google.gms:google-services                   | 4.3.5 | 4.3.10 | 升级                   |
| de.timfreiheit.resourceplaceholders:placeholders | 0.4   | 0.3    | 0.4 minGradle=6.7.1    |

### 2. `app/build.gradle`

| 编译              | 版本   | 修改为 | 备注 |
| ----------------- | ------ | ------ | ---- |
| compileSdkVersion | 30     | 30     | 默认 |
| buildToolsVersion | 30.0.3 | 30.0.3 | 默认 |
| minSdkVersion     | 21     | 19     |      |
| targetSdkVersion  | 30     | 30     | 默认 |

| 包                                            | 版本   | 修改为 | 备注                          |
| --------------------------------------------- | ------ | ------ | ----------------------------- |
| androidx.core:core-ktx                        | 1.6.0  | 1.3.2  | 1.6.0 minSdk=21               |
| androidx.constraintlayout:constraintlayout    | 2.1.0  | 2.1.1  | 升级                          |
| androidx.media:media                          | 1.4.1  | 1.4.3  | 1.4+ minCompileSdk=30         |
| $exoplayer_version                            | 2.15.0 | 2.15.1 | 升级                          |
| com.google.android.exoplayer:extension-okhttp | 2.15.0 | 2.14.2 | 2.15.0 okhttp=4.9.1 minSdk=21 |
| com.squareup.okhttp3:okhttp                   | 无     | 3.12.0 | 3.13+ minSdk=21               |

### 3. `basemvplib/build.gradle`

| 编译              | 版本   | 修改为 | 备注 |
| ----------------- | ------ | ------ | ---- |
| compileSdkVersion | 30     | 30     | 默认 |
| buildToolsVersion | 30.0.3 | 30.0.3 | 默认 |
| minSdkVersion     | 19     | 19     | 默认 |
| targetSdkVersion  | 30     | 30     | 默认 |

| 包                           | 版本  | 修改为 | 备注            |
| ---------------------------- | ----- | ------ | --------------- |
| androidx.core:core-ktx       | 无    | 1.3.2  | 1.6.0 minSdk=21 |
| androidx.appcompat:appcompat | 1.3.0 | 1.3.1  | 升级            |

## 代码

根路径：`app/src/main/java/com/kunfei/bookshelf`

### 1. `response`

okhttp3 response属性为私有改回方法

`./help/glide/OkHttpStreamFetcher.kt`

| 代码             | 改回               |
| ---------------- | ------------------ |
| response.body    | response.body()    |
| response.message | response.message() |
| response.code    | response.code()    |

### 2. `WebDav`

下列代码需要 okhttp3 4.0+，改回旧代码

`./utils/webdav/WebDav.kt`

```java
import okhttp3.MediaType.Companion.toMediaType
import okhttp3.RequestBody.Companion.asRequestBody
import okhttp3.RequestBody.Companion.toRequestBody

toRequestBody() => RequestBody.create();
asRequestBody() => RequestBody.create();
.toMediaType() => MediaType.parse();
```

### 3.`不支持的API`

一些代码不支持 minSdk 19，进行修改

| 代码                              | minSdk | 相对路径                                                     |
| --------------------------------- | ------ | ------------------------------------------------------------ |
| Intent.ACTION_OPEN_DOCUMENT_TREE  | 21     | ./help/storage/BackupRestoreUi.kt                            |
| FLAG_DRAWS_SYSTEM_BAR_BACKGROUNDS | 21     | ./utils/ActivityExtensions.kt                                |
| statusBarColor                    | 21     | ./utils/ActivityExtensions.kt<br />./view/activity/ReadBookActivity.java |
| navigationBarColor                | 21     | ./utils/ActivityExtensions.kt<br />/view/activity/ReadBookActivity.java |

# 功能增加

## 书签备份恢复

```
./help/storage/Backup.kt
./help/storage/Restore.kt
```

