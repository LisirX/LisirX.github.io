---
title: Android 加解密App
mathjax: false
tags: ['Android', 'APP', 'Kotlin']
categories: ['Android']
copyright: true
date: 2022-01-31 08:51:09
---

写这个项目的原因，是因为我有一个神奇的算法来加解密字符串，已经用java在电脑客户端实现了。现在打算在android上实现一下，主要是为了学习android开发，顺便学习kotlin语言。

<!-- more -->

# 到Android官网查看官方文档，学习Android开发

[应用基础知识](https://developer.android.com/guide/components/fundamentals)。

## 学习Android

### 开发语言

> 您可以使用 Kotlin、Java 和 C++ 语言编写 Android 应用。Android SDK 工具会将您的代码连同任何数据和资源文件编译成一个 APK（Android 软件包），即带有 `.apk` 后缀的归档文件。一个 APK 文件包含 Android 应用的所有内容，它也是 Android 设备用来安装应用的文件。

那么本次我打算使用的开发语言是`Kotlin`。`Kotlin`成为Google Android的开发**首选语言**，并且开发较为easy，所以学习它是有必要的。

### 软件是如何运行在Android的

> 每个 Android 应用都处于各自的安全沙盒中，并受以下 Android 安全功能的保护：
> 
> Android 操作系统是一种多用户 Linux 系统，其中的每个应用都是一个不同的用户；
> 
> 默认情况下，系统会为每个应用分配一个唯一的 Linux 用户 ID（该 ID 仅由系统使用，应用并不知晓）。系统会为应用中的所有文件设置权限，使得只有分配给该应用的用户 ID 才能访问这些文件；
> 
> 每个进程都拥有自己的虚拟机 (VM)，因此应用代码独立于其他应用而运行。
> 
> 默认情况下，每个应用都在其自己的 Linux 进程内运行。Android 系统会在需要执行任何应用组件时启动该进程，然后当不再需要该进程或系统必须为其他应用恢复内存时，其便会关闭该进程。

这里介绍的是android应用运行的情况。可以看到，android在运行时相互独立。

> Android 系统实现了最小权限原则。换言之，默认情况下，每个应用只能访问执行其工作所需的组件，而不能访问其他组件。这样便能创建非常安全的环境，在此环境中，应用无法访问其未获得权限的系统部分。不过，应用仍可通过一些途径与其他应用共享数据以及访问系统服务：
> 
> 可以安排两个应用共享同一 Linux 用户 ID，在此情况下，二者便能访问彼此的文件。为节省系统资源，也可安排拥有相同用户 ID 的应用在同一 Linux 进程中运行，并共享同一 VM。应用还必须使用相同的证书进行签名。
> 
> 应用可以请求访问设备数据（如用户的联系人、短信消息、可装载存储装置（SD 卡）、相机、蓝牙等）的权限。用户必须明确授予这些权限。如需了解详细信息，请参阅[使用系统权限](https://developer.android.com/training/permissions)。

这里指出了“权限”的概念。这个概念在普通的android应用中非常常见。平时随便下载一个什么应用就需要什么什么权限（比如照相机等等）

### 应用组件-Activity

在很多android上都可以看到Activity的身影。那么到底什么是Activity呢？

> Activity 是与用户交互的入口点。它表示拥有界面的单个屏幕。

官网上举出了几个例子

> 例如，电子邮件应用可能有一个显示新电子邮件列表的 Activity、一个用于撰写电子邮件的 Activity 以及一个用于阅读电子邮件的 Activity。尽管这些 Activity 通过协作在电子邮件应用中形成一种紧密结合的用户体验，但每个 Activity 都独立于其他 Activity 而存在。因此，其他应用可以启动其中任何一个 Activity（如果电子邮件应用允许）。例如，相机应用可以启动电子邮件应用内用于撰写新电子邮件的 Activity，以便用户共享图片。

这个Activity有以下好处

1. 追踪用户当前关心的内容（屏幕上显示的内容），以确保系统继续运行托管 Activity 的进程。
2. 了解先前使用的进程包含用户可能返回的内容（已停止的 Activity），从而更优先保留这些进程。
3. 帮助应用处理终止其进程的情况，以便用户可以返回已恢复其先前状态的 Activity。
4. 提供一种途径，让应用实现彼此之间的用户流，并让系统协调这些用户流。（此处最经典的示例是共享。）

如何使用Activity？您需将 Activity 作为 `Activity` 类的子类来实现。如需了解有关 `Activity` 类的更多信息，请参阅 [Activity](https://developer.android.com/guide/components/activities) 开发者指南。

我一打开这个的开发者指南，一堆英文：生命周期，还有`Activity`中一些方法的含义。

啊喂！！我是初学者！先搞一个小玩意出来玩玩！！再深入了解一下它的实现过程！！不香嘛！

所以果断跳过，看到[布局](https://developer.android.com/guide/topics/ui/declaring-layout)

> 布局定义了应用中的界面结构（例如 Activity 的界面结构）。布局中的所有元素均使用 `View` 和 `ViewGroup` 对象的层次结构进行构建。`View` 通常用于绘制用户可看到并与之交互的内容。`ViewGroup` 则是不可见的容器，用于定义 `View` 和其他 `ViewGroup` 对象的布局结构，如图 1 所示。
> 
> ![图1. 视图层次结构的图示，它定义了一个界面布局](Android-加解密App/viewgroup_2x.png)
> 
> `View` 对象通常称为“微件”，可以是多个子类之一，例如 `Button` 或 `TextView`。`ViewGroup` 对象通常称为“布局”，可以是提供不同布局结构的众多类型之一，例如 `LinearLayout` 或 `ConstraintLayout`。
> 
> 您可通过两种方式声明布局：
> 
> 在 XML 中声明界面元素。Android 提供对应 View 类及其子类的简明 XML 词汇，如用于微件和布局的词汇。
> 您也可使用 Android Studio 的 Layout Editor，并采用拖放界面来构建 XML 布局。
> 
> 在运行时实例化布局元素。您的应用能以程序化方式创建 `View` 对象和 `ViewGroup` 对象（并操纵其属性）。
> 通过在 XML 中声明界面，您可以将应用外观代码与控制其行为的代码分开。使用 XML 文件还有助于为不同屏幕尺寸和屏幕方向提供不同布局（支持不同的屏幕尺寸中深入阐述了此内容）。
> 
> 借助 Android 框架，您可以灵活选择使用两种或其中一种方法来构建应用界面。例如，您可以在 XML 中声明应用的默认布局，然后在运行时修改布局。

对我来说，进行布局，当然是用XML来的方便并且更加便于调试和修改（再说，有Layout Editor）。而关于如何编写XML：参阅[布局资源](https://developer.android.com/guide/topics/resources/layout-resource)文档

### 加载 XML 资源

> 当您编译应用时，系统会将每个 XML 布局文件编译成 `View` 资源。
> 您应在 `Activity.onCreate()` 回调实现内加载应用代码中的布局资源。
> 通过调用 `setContentView()`，并以 `R.layout.layout_file_name` 形式向应用代码传递对布局资源的引用，您即可执行此操作。
> 例如，如果您的 XML 布局保存为 main_layout.xml，您应通过如下方式为 `Activity` 加载布局资源：
> 
``` Kotlin
fun onCreate(savedInstanceState: Bundle) {
    super.onCreate(savedInstanceState)
    setContentView(R.layout.main_layout)
}
```
> 启动 Activity 时，Android 框架会调用 `Activity` 中的 `onCreate()` 回调方法。

而在我新建的一个项目中，自动生成了这些代码(MainActivity.kt)
``` Kotlin
package lisir.ningbo.encrypto

import android.os.Bundle
import com.google.android.material.snackbar.Snackbar
import androidx.appcompat.app.AppCompatActivity
import androidx.navigation.findNavController
import androidx.navigation.ui.AppBarConfiguration
import androidx.navigation.ui.navigateUp
import androidx.navigation.ui.setupActionBarWithNavController
import android.view.Menu
import android.view.MenuItem
import lisir.ningbo.encrypto.databinding.ActivityMainBinding

class MainActivity : AppCompatActivity() {

    private lateinit var appBarConfiguration: AppBarConfiguration
    private lateinit var binding: ActivityMainBinding

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)

        binding = ActivityMainBinding.inflate(layoutInflater)
        setContentView(binding.root)

        setSupportActionBar(binding.toolbar)

        val navController = findNavController(R.id.nav_host_fragment_content_main)
        appBarConfiguration = AppBarConfiguration(navController.graph)
        setupActionBarWithNavController(navController, appBarConfiguration)

        binding.fab.setOnClickListener { view ->
            Snackbar.make(view, "Replace with your own action", Snackbar.LENGTH_LONG)
                .setAction("Action", null).show()
        }
    }

    override fun onCreateOptionsMenu(menu: Menu): Boolean {
        // Inflate the menu; this adds items to the action bar if it is present.
        menuInflater.inflate(R.menu.menu_main, menu)
        return true
    }

    override fun onOptionsItemSelected(item: MenuItem): Boolean {
        // Handle action bar item clicks here. The action bar will
        // automatically handle clicks on the Home/Up button, so long
        // as you specify a parent activity in AndroidManifest.xml.
        return when (item.itemId) {
            R.id.action_settings -> true
            else -> super.onOptionsItemSelected(item)
        }
    }

    override fun onSupportNavigateUp(): Boolean {
        val navController = findNavController(R.id.nav_host_fragment_content_main)
        return navController.navigateUp(appBarConfiguration)
                || super.onSupportNavigateUp()
    }
}
```
里面`onCreate()`函数有
``` Kotlin
binding = ActivityMainBinding.inflate(layoutInflater)
setContentView(binding.root)
```

这是啥！？

看了[视图绑定（ViewBinding ）与数据绑定（Databinding）](https://blog.csdn.net/weixin_37873786/article/details/120520482)
> 为某个模块启用视图绑定功能后，系统会为该模块中包含的每个 XML 布局文件生成一个绑定类。
> 每个绑定类均包含对根视图以及具有 ID 的所有视图的引用。系统会通过以下方式生成绑定类的名称：将 XML 文件的名称转换为驼峰式大小写，并在末尾添加“Binding”一词。

这不就是把XML文件变了个binding跑出来嘛！

然而，在我真正要去实现一个APP的时候，可以从最简单开始，先创建了一个Empty Activity，再自行构建xml。这不就简单很多了！

## 分析创建项目时IDE自动生成的代码，温习知识

```Kotlin
class MainActivity : AppCompatActivity() {
    override fun onCreate(savedInstanceState: Bundle?) { // 每次生成Activity时都会运行这个函数
        super.onCreate(savedInstanceState) // 这个Bundle暂时理解为传输数据的东西？
        setContentView(R.layout.activity_main) // R中储存着自动生成的一些视图，也就是activity_main.xml的视图。
		// 习惯上Activity的改驼峰式命名为xml的下划线式命名。
		// setContentView() Doc: 
		// Set the activity content from a layout resource. 
		// The resource will be inflated, adding all top-level views to the activity.
    }
}
```

# 进入项目

## 首先：加密算法

这个加密算法就用`Code0.EnCode(String s, String pswd)`表示加密，`Code0.DeCode(String s, String pswd)`表示解密吧。
这里主要不是为了加密算法，所以略过加密解密算法的实现过程。
`String s, String pswd`中的`s`是待加解密的字符串，`pswd`是密钥
在解密过程中如果遇到密文和密钥不对应的情况，会抛出`CryptoException`

## 其次：activity_main.xml设计

这里我直接使用了Android Studio的拖动设计
```
<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:id="@+id/key"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".MainActivity">

    <EditText
        android:id="@+id/PlainText"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:ems="10"
        android:hint="@string/PlainText"
        android:gravity="start|top"
        android:inputType="textMultiLine"
        android:minHeight="48dp"
        android:minLines="3"
        android:maxLines="3"
        app:layout_constraintBottom_toTopOf="@+id/CipherText"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintHorizontal_bias="0.5"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="parent" />

    <EditText
        android:id="@+id/CipherText"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:ems="10"
        android:hint="@string/CipherText"
        android:gravity="start|top"
        android:inputType="textMultiLine"
        android:minHeight="48dp"
        android:minLines="3"
        android:maxLines="3"
        app:layout_constraintBottom_toTopOf="@+id/KeyText"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintHorizontal_bias="0.5"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toBottomOf="@+id/PlainText" />

    <EditText
        android:id="@+id/KeyText"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:ems="10"
        android:hint="@string/KeyText"
        android:inputType="text"
        android:minHeight="48dp"
        app:layout_constraintBottom_toTopOf="@+id/Switch"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintHorizontal_bias="0.5"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toBottomOf="@+id/CipherText" />

    <Button
        android:id="@+id/Generate"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="@string/Generate"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintHorizontal_bias="0.5"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toBottomOf="@+id/Switch" />

    <Switch
        android:id="@+id/Switch"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:minHeight="48dp"
        android:text="@string/DecodeMode"
        app:layout_constraintBottom_toTopOf="@+id/Generate"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintHorizontal_bias="0.5"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toBottomOf="@+id/KeyText" />

</androidx.constraintlayout.widget.ConstraintLayout>
```
下次一定有空要把xml文件格式学会

## 在Activity中创建一些监听等等
```
Generate.setOnClickListener { // 监听按钮操作
    val plainText = PlainText.text.toString()
    val cipherText = CipherText.text.toString()
    val keyText = KeyText.text.toString()
    when (Switch.isChecked) {
        true -> {
            // 解密的Switch: Chosen
            try {
                PlainText.setText(Code0.DeCode(cipherText, keyText))
            } catch (e : Exception) {
                Toast.makeText(applicationContext, R.string.WrongKey, Toast.LENGTH_SHORT).show()
				// 提示用户R.string.WrongKey
				// 这里的R也是自动生成的资源类，使用了String.xml
            }
        }
        false -> {
            // 解密的Switch: Not Chosen 也就是加密
            CipherText.setText(Code0.EnCode(plainText, keyText))
        }
    }
}
```

---

# MainActivity.kt

```Kotlin
package lisir.ningbo.encrypto

import Tools.Code0
import androidx.appcompat.app.AppCompatActivity
import android.os.Bundle
import android.widget.Toast
import kotlinx.android.synthetic.main.activity_main.*
import kotlinx.android.synthetic.main.activity_main.view.*
import java.lang.Exception

class MainActivity : AppCompatActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)
        Generate.setOnClickListener {
            val plainText = PlainText.text.toString()
            val cipherText = CipherText.text.toString()
            val keyText = KeyText.text.toString()
            when (Switch.isChecked) {
                true -> {
                    // Chosen
                    try {
                        PlainText.setText(Code0.DeCode(cipherText, keyText))
                    } catch (e : Exception) {
                        Toast.makeText(applicationContext, R.string.WrongKey, Toast.LENGTH_SHORT).show()
                    }
                }
                false -> {
                    // Not Chosen
                    CipherText.setText(Code0.EnCode(plainText, keyText))
                }
            }
        }
    }

}
```
