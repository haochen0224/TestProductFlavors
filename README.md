# TestProductFlavors

本项目是为了测试 gradle 多渠道打包中的 matchingFallbacks 和 missingDimensionStrategy 配置。



## 如何使用

### 1. 如何配置matchingFallbacks

在build.gradle的 productFlavors 的某个风味下添加matchingFallbacks配置，例如：

```groovy
productFlavors {
    cnbeta {
        dimension "DOMAIN"
        matchingFallbacks = ['china', 'oversea']
    }
    china {
        dimension "DOMAIN"
        matchingFallbacks = [ 'cnbeta', 'oversea']
    }
    oversea {
        dimension "DOMAIN"
        matchingFallbacks = ['china', 'cnbeta']
    }
}
```

### 2. 如何配置missingDimensionStrategy

在build.gradle的defaultConfig下添加 missingDimensionStrategy 配置，例如：

```groovy
    defaultConfig {
        minSdkVersion 24
        targetSdkVersion 33

        testInstrumentationRunner "androidx.test.runner.AndroidJUnitRunner"
        consumerProguardFiles "consumer-rules.pro"

        missingDimensionStrategy 'DOMAIN', 'china', 'cnbeta', 'oversea'
    }
```

### 3. 多风味打包

打包时选择 build variants:

![1](https://github.com/haochen0224/TestProductFlavors/blob/main/readmeImages/1.png?raw=true)

### 4. 如何判断某个目录下的资源和代码被正确打包了？

每种风味的代码目录下面有个 color 资源：

![2](https://github.com/haochen0224/TestProductFlavors/blob/main/readmeImages/2.png?raw=true)

该资源的 name 指明了其在哪个模块的哪个目录下，比如：

![3](https://github.com/haochen0224/TestProductFlavors/blob/main/readmeImages/3.png?raw=true)

该color资源在 device 模块的 china 目录下。

在打包生成的 apk 中：

![4](https://github.com/haochen0224/TestProductFlavors/blob/main/readmeImages/4.png?raw=true)

我们可以通过查看被打包进去的color资源的 name，来判断哪个目录下的代码和资源被打包了，以用来验证 matchingFallbacks 和 missingDimensionStrategy 的配置是否正确。
