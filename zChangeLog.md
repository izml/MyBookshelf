# 版本降级(minSdk=19)

## build文件

### 1. `build.gradle`

| 包                                               | 版本  | 降级为 | 备注                   |
| ------------------------------------------------ | ----- | ------ | ---------------------- |
| com.android.tools.build:gradle                   | 7.0.2 | 1.3.2  | 4.2.0+ minGradle=6.7.1 |
| com.google.gms:google-services                   | 4.3.5 | 4.3.10 | 升级                   |
| de.timfreiheit.resourceplaceholders:placeholders | 无    | 1.3.1  | 0.4+ minGradle=6.7.1   |

### 2. `app/build.gradle`

| 编译              | 版本   | 降级为 | 备注 |
| ----------------- | ------ | ------ | ---- |
| compileSdkVersion | 30     | 29     | 默认 |
| buildToolsVersion | 30.0.3 | 29.0.3 |      |
| minSdkVersion     | 21     | 19     |      |
| targetSdkVersion  | 30     | 29     | 默认 |

| 包                                            | 版本   | 降级为 | 备注                          |
| --------------------------------------------- | ------ | ------ | ----------------------------- |
| androidx.core:core-ktx                        | 无     | 1.3.2  | 1.6.0 minSdk=21               |
| androidx.media:media                          | 无     | 1.3.1  | 1.4+ minCompileSdk=30         |
| com.google.android.exoplayer:extension-okhttp | 2.15.0 | 2.14.2 | 2.15.0 okhttp=4.9.1 minSdk=21 |
| com.squareup.okhttp3:okhttp                   | 无     | 3.12.0 | 3.13+ minSdk=21               |

### 3. `basemvplib/build.gradle`

| 编译              | 版本   | 降级为 | 备注 |
| ----------------- | ------ | ------ | ---- |
| compileSdkVersion | 30     | 29     |      |
| buildToolsVersion | 30.0.3 | 29.0.3 |      |
| minSdkVersion     | 19     | 19     |      |
| targetSdkVersion  | 30     | 29     |      |

| 包                           | 版本  | 降级为 | 备注            |
| ---------------------------- | ----- | ------ | --------------- |
| androidx.core:core-ktx       | 无    | 1.3.2  | 1.6.0 minSdk=21 |
| androidx.appcompat:appcompat | 1.3.0 | 1.3.1  | 升级            |

## 代码

### 1. `response`

okhttp3 response属性为私有改回方法

`app/src/main/java/com/kunfei/bookshelf/help/glide/OkHttpStreamFetcher.kt`

| 代码             | 改回               |
| ---------------- | ------------------ |
| response.body    | response.body()    |
| response.message | response.message() |
| response.code    | response.code()    |

### 2. `WebDav`

需要 okhttp3 4.0+，改回旧代码

`app/src/main/java/com/kunfei/bookshelf/utils/webdav/WebDav.kt`

```java
import okhttp3.MediaType.Companion.toMediaType
import okhttp3.RequestBody.Companion.asRequestBody
import okhttp3.RequestBody.Companion.toRequestBody

toRequestBody() => RequestBody.create();
asRequestBody() => RequestBody.create();
.toMediaType() => MediaType.parse();
```

# 功能增加

## 书签备份恢复

```
app/src/main/java/com/kunfei/bookshelf/help/storage/Backup.kt
app/src/main/java/com/kunfei/bookshelf/help/storage/Restore.kt
```

