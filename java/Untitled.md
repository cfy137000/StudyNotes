# Android Pie
## 首要影响

### 1. 对非SDK接口的限制
可以使用AOSP中的静态分析工具 veridex来进行检测https://android.googlesource.com/platform/prebuilts/runtime/+/master/appcompat

### 2. 删除Crypto provider

### 3. 严格的UTF-8编码
在使用new String(byte[]) 这种形式来构建String的时候,默认的编码格式为utf-8,但是要比以往更加严格,之前的utf-8编码的扩展都不再支持,所以应当检查代码中是否有通过字节数组构建字符串地地方

### 4. 访问硬件传感器

### 5. 前台服务

### 6. Bouncy Castle密码的弃用

### 7. 删除Build.getSerial()

### 8. 不允许共享WebView数据目录

### 9. 无法通过路径访问其他应用文件夹


## 次要影响

### 1.FLAG_ACTIVITY_NEW_TASK 要求
在非Activity中启动应用均需要这只该FLAG,这是AndroidN上提出的,但是由于Google的原因,一直没有实装,现在实装了,在App中,基本上已经对应完了,所以只需要关注一下,以防遗漏