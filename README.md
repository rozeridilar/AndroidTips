# Android tips & tricks ⚡️

## [#1 Proguard](https://rozeridilar.com/2019/12/27/proguard/)

<strong>ProGuard -- shrinking, optimization, obfuscation, and preverification of Java bytecode.</strong>

<strong>Proguard</strong> optimizes the bytecode, removes unused codes, and obfuscates the classes, fields, methods with shorter names. Optimization operates with java bytecode. Since android runs on Dalvik bytecode which is converted from java bytecode, some optimizations usually doesn't work well ( So you should be careful ).

The obfuscated code makes your APK difficult to reverse engineer, which is the main reason why most of the developers choose proguard.

Proguard can sometimes break your code up since it renames almost everything. Be sure to thoroughly test your app before release, especially after changing proguard config.

<strong>Prevent Obfuscation In Some Classes</strong>

Every app has some kind of data classes, models, or some important classes which they cannot be obfuscated. In such circumstances, we cannot let proguard to rename or remove any fields of those classes. It's a safe bet to add a @Keep annotation on the whole class or methods, or a wildcard rule on proguard-rules.pro.

<strong>1 - @Keep Annotation:</strong>

Denotes that the annotated element should not be renamed when the code is minified at build time. This is typically used on methods and classes that are accessed only via reflection so a compiler may think that the code is unused.
<pre><code>@Keep
  public void foo() {
      ...
  }
 </code></pre>
<strong>Note that:</strong> This annotation is available only when using the Annotations Support Library.

<strong>2 - Using ProGuard, keep class fields with wildcard:</strong>
<pre class="default prettyprint prettyprinted"><code><span class="pun">-</span><span class="pln">keepclassmembers </span><span class="kwd">class</span><span class="pln"> com</span><span class="pun">.</span><span class="kwd">my</span><span class="pun">.</span><span class="kwd">package</span><span class="pun">.**</span> <span class="pun">{</span>
    <span class="kwd">public</span> <span class="kwd">protected</span> <span class="str"><fields></span><span class="pun">;</span>
    <span class="kwd">private</span> <span class="pun">***</span> <span class="kwd">string</span><span class="pun">*;</span>
<span class="pun">}</span></code></pre>
<strong>Tips & Tricks in Proguard</strong>
<ul>
 	<li>Proguard does not obfuscate the class names and public methods called in AndroidManifest.xml by default. So you don't have to add 'keep'in those classes.</li>
 	<li>Android Analyser can help you a lot while deciding which rules to add in proguard. Yet, to prevent unexpected crashes, be sure to test your release app in depth at the end.</li>
 	<li>To create released apk, change your directory in terminal then add the following snippet and run.
<pre class="default prettyprint prettyprinted"><span style="color:#222222;font-family:monospace;"><span style="background-color:#e9ebec;">./gradlew assembleRelease</span></span></pre>
</li>
 	<li>Finally If you are building library and want to change your release lib name, add the following line to your build-gradle(module: app) in android {} part.
<pre class="default prettyprint prettyprinted"><code><span class="pln">project</span><span class="pun">.</span><span class="pln">archivesBaseName </span><span class="pun">=</span> <span class="str">"SomeExample"</span><span class="pun">;
</span></code></pre>
<p class="p4"><span class="s1">Some useful Q&As:</span></p>
<p class="p3"><span class="s1"><a href="https://stackoverflow.com/questions/35321742/android-proguard-most-aggressive-optimizations">https://stackoverflow.com/questions/35321742/android-proguard-most-aggressive-optimizations</a></span></p>
<p class="p3"><span class="s1"><a href="https://stackoverflow.com/questions/33958972/how-to-obfuscate-everything-but-public-method-names-and-attributes-with-proguard">https://stackoverflow.com/questions/33958972/how-to-obfuscate-everything-but-public-method-names-and-attributes-with-proguard</a></span></p>
<p class="p3"><span class="s1"><a href="https://stackoverflow.com/questions/30526173/obfuscate-private-fields-using-proguard">https://stackoverflow.com/questions/30526173/obfuscate-private-fields-using-proguard</a></span></p>

</li>
</ul>

## [#2 baselineAligned](https://rozeridilar.com/2020/03/11/baselinealigned-2/)

"Defines whether widgets contained in this layout are baseline-aligned or not."

By setting <code>android:baselineAligned="false"</code> , app prevents the layout from aligning its children's baselines, that means app doesn't worry about where the baseline of other elements in the layout, which increases the UI performance.

<strong>Note:</strong> By default, <code>baselineAligned</code> is set to <code>true</code>.


## [#3 Publishing Libraries in Jitpack](https://rozeridilar.com/2020/03/11/publishing-libraries-on-jitpack/)

Publishing Single Library on Jitpack

JitPack is a novel package repository for JVM and Android projects. It builds Git projects on demand and provides you with ready-to-use artifacts (jar, aar).
1. Create a new android project.
1. Add your library by File -> New -> Import a module.
1. Add the JitPack maven repository to the list of repositories:
```java
repositories {
jcenter() maven { url "https://jitpack.io" }
}
```

Note: when using multiple repositories in build.gradle it is recommended to add JitPack at the end. Gradle will go through all repositories in order until it finds a dependency.

4. Then publish your new project to your GitHub account. Then, push your first version tag.
Note: You have to give permission from your account to jitpack.
single library example

Publishing Multiple Libraries On Jitpack

1. Create a new android project.
1. Add your libraries one by one by File -> New -> Import a module.
1. Add the JitPack maven repository to the list of repositories:
```java
repositories {
jcenter() maven { url "https://jitpack.io" }
}
```

Note: when using multiple repositories in build.gradle it is recommended to add JitPack at the end. Gradle will go through all repositories in order until it finds a dependency.

4. Then publish your new project to your GitHub account. Then, push your first version tag.
Note: You have to give permission from your account to jitpack.
example for multiple libraries

**Important Notes** 🚀
Lets assume you have module1, module2, module3 in your android project.
To share only one module, you can add:
implementation ‘com.github.yourproject:module1:module1’sTAG'
Or to share all your project directly:
implementation’com.github.yourproject:yourproject'sTAG'

If you are using apply plugins for such as androidanalyser, the developer who wants to use your project _must_ embed it in her/his app. So be sure, that you _embed as few plugins as possible_.
