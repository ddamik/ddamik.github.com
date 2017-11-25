# 그레들 파일
안드로이드에서는 빌드 작업을 그레들이 담당한다.   
build.gradle 파일에서 그레들 관련 설정을 할 수 있다. build.gradle은 'GradleScripts' 영역에 있다.

gradle파일은 프로젝트 수준과 모듈 수준으로 구분되는데, *앱*이 모듈 단위이며, *여러 모듈을 묶어서 관리*하기 위한 것이 프로젝트 개념이다.

## settings.gradle
그레들 모듈을 포함하여 그레들이 모듈을 관리하고 빌드하게 설정하는 파일이다.

## 프로젝트 수준의 그레들
Gradle Scripts 영역의 최상위에 있는 build.gradle로, 옆에 프로젝트명이 표시된 파일이다.  
모든 모듈을 위한 최상위 설정을 목적으로 한다.  
```xml
buildscript {
    repositories {
        jcenter()
    }
    dependencies {
        classpath: 'com.android.tools.build:gradle:30.0.0-alpha8'
    }
}

allprojects {
    repositories {
        jcenter()
    }
}

task clean(type: Delete) {
    delete rootProject.buildDir
}
```


## 모듈 수준의 그레들
개발자가 그레들을 설정한다면 대부분 모듈 수준의 그레들이다.
```xml
apply plugin: 'com.android.application'

android {
    complieSdkVersion 26
    buildToolsVersion "26.0.1"
    defaultConfig {
        applicationId "com.sixhustle.ddamik.androidlab"
        minSdkVersion 15
        targetSdkVersion 26
        versionCode 1
        versionName "1.0"
        testInstrumentationRunner "android.support.test.runner.AndroidJUnitRunner"
    }
    buildTypes {
    release {
        minifyEnabled false
        proguardFiles getDefaultProguardFile('proguard-android.txt'), 'proguard-rules.pro'
    }
}

dependencies {
    ...
    // 치기 너무 귀찮..
}
```

- complieSdkVersion: 사용하는 컴파일 버전
- buildToolsVersion: 사용하는 빌드 툴 버전

외부 라이브러리 코드를 사용할 때, 위의 빌드 정보와 충돌발생으로 빌드 실패할 수 있다.      
이럴때, 그레들 파일을 수정하여 빌드 툴 버전을 명시하거나 sdk 매니저를 이용해 맞는 버전의 빌드 툴을 추가 설치해야 한다.

<hr/>

- applicationId "com.sixhustle.ddamik.androidlab": 앱의 식별자
- minSdkVersion: 최소 지원 범위
- targetSdkVersion: 사용하고 있는 SDK 버전
- versionCode: 앱의 버전

다른 앱과 똑같은 applicationId 값으면 구글 Play 스토어에 등록되지 않는다.

<hr/>

- dependencies: 앱을 위한 라이브러리 등록
외부 라이브러리를 사용하려면 그레들 파일의 dependencies에 등록해야 빌드가 정상적으로 이뤄진다.