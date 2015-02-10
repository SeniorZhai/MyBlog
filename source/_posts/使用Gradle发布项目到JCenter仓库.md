title: 使用Gradle发布项目到JCenter仓库
date: 2015-02-04 16:47:29
categories: Android 
tags: [AS,JCenter,Gradle]
---
`JCenter`是Android Studio中repositories的默认节点。
<!--more-->
##申请Bintray账号
需要在[Bintray](https://bintray.com/)注册一个账号
##生成项目的JavaDoc和source JARs
JCenter需要我上传远程仓库以上两个文件，这一步需要`android-maven-plugin`插件，所以在项目中的`build.gragle`(项目最外层)构建依赖
```
buildscript{
	repositories {
		jcenter();
	}
	dependencies {
		classpath 'com.android.tools.build:gradle:1.0.0'
		classpath 'com.github.dcendents:android-maven-plugin:1.2'
	}
	allprojects {
		repositories {
			jcenter()
		}
	}
}
```
然后在需要发布的那个module的`build.gradle`里配置修改
```
apply plugin: 'com.android.library'
apply plugin: 'com.github.dcendents.android-maven'
apply plugin: 'com.jfrog.bintray'

version = "1.0.0"
android{
	compileSdkVersion 21
	buildToolsVersion "21.1.2"
	resourcePrefix "resourcePrefix" // 这个随便填
	defaultConfig {
		minSdkVersion 9
		targetSdkVersion 21
		versionCode 1
		versionName version
	}
	buildTypes {
		release {
			minifyEnabled false
			proguardFiles getDefaultProguardFile('proguard-android.txt'),'proguard-rules.pro'
		}
	}
}
dependencies {
	compile fileTree(dir: 'libs',include: ['*.jar'])
	compile 'com.nineoldandroid:library:2.4.0+'
}
def siteUrl = 'https://github.com/XXX/XXX'	// 项目的主页
def gitUrl = 'https://github.com/XXX/XXX.git'	// git仓库URL
install {
	repositories.mavenInstaller {
		pom {
			project {
				packaging 'arr'
				name 'Project'	// 项目名
				url siteUrl
				licenses {		// 协议
					license {
						name 'The Apache Software License, Version 2.0'
						url 'http://www.apache.org/licenses/LICENSE-2.0.txt'
					}
				}
				developers {
					developer {	// 基本信息
						id 'seniorzhai'
						name 'zhaitao'
						email 'developer.zhaitao@gmail.com'
					}
				}
				scm {
					connection gitUrl
					developerConnection gitUrl
					url siteUrl
				}
			}
		}
	}
}
task sourcesJar(type: Jar) {
	from android.sourceSets.main.java.srcDirs
	classifier = 'sources'
}
task javadoc(type: Javadoc) {
	source = android.sourceSets.main.java.srcDirs
	classpath += project.files(android.getBootClassPath().join(File.pathSeparator))
}
task javadocJar(type: Jar,dependsOn: javadoc) {
	classifier = 'javadoc'
	from javadoc.desinationDir
}
artifacts {
	archiver javadocJar
	archiver sourcesJar
}
Properties properties = new Properties()
properties.load(project.rootProject.file('local.properties'),newDataInputStream())
bintray {
	user = properties.getProperty("bintray.user")
	key = properties.getProperty("bintray.apikey")
	configurations = ['archives']
	pkg {
		repo = 'maven'
		name = "ProjectName"
		websiteUrl = siteUrl
		vcsUrl = gitUrl
		licenses = ["Apache-2.0"]
		publish = true
	}
}
```
上述的`local.properties`文件在项目的根目录，用于配置`bintray`账号信息，这个文件需要`gitignore`防止泄露账号信息
```
bitray.user=your_user_name
bitray.apokey=yor_apikey
```
Rebuild一下项目，module里的build文件夹会生成相应文件，项目生成到本地仓库，Android Studio默认在`Android\sdk\extras\android\m2repository`文件夹中
执行`gradlew install`
##上传到Bintray
上传需要`gradle-bintray-plugin`的支持
在`build.gradle`里构建
```
dependencies {
	...
	classpath 'com.jfrog.bintray.gradle:gradle-bintray-plugin:1.0'
	...
}
```
再`Rebuild`下，然后执行`gradlew bintrayUpload`
上传完成后就可再Bintray上找到Repo，最后申请Repo添加到JCenter，在<https://bintray.com/bintray/jcenter>输入项目的名字点击匹配到的项目，然后写一写Comments再send即可，等管理员审批即可。
成功后，在其他项目中调用添加denpendcies即可