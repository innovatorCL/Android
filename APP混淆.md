## 变态混淆

  Android直接使用Proguard进行的是常规abcd等混淆方式对逆向的干扰程序并不是很大，所以需要一个变态的字典，测试过符号类型的字典（美团APP），但由于编码问题，在各个平台上反编译时显示的效果也不同，windows上显示为汉字，干扰性还是一般，于是有了这个智障级混淆字典。 使用 ： 

- 在Android工程目录的app module模块下，也就是与proguard-project.txt在同一目录中放置dic.txt混淆字典文件； 

- 根据自己需要在proguard-project.txt文件中进行配置： -obfuscationdictionary dic.txt -classobfuscationdictionary dic.txt -packageobfuscationdictionary dic.txt 

- 使用gradlew clean --->再打包； 4.提取apk中的dex文件，使用dex2jar工具反编译--->再使用jd-gui工具阅读反编译的jar文件，发现类名和方法名都变成了000000，说明成功混淆。
