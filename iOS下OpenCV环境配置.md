# iOS下OpenCV环境配置

现在在iOS上使用OpenCV已经方便很多, 可以直接到官网下载openCV.Framework, 记得自己在以前2.0的时候还要自己下载项目编译.

### 1. 首先去官网下载opencv3.2.0的framework
[OpenCV官网](http://opencv.org/)

### 2. 导入工程

首先引入framework, 然后再添加头文件**#import \<opencv2/opencv.hpp\>**

在这里有几个注意的地方
在***opencv2.framework/.../exposure_compensate.hpp***有写到:

```cpp
#if defined(NO)
#  warning Detected Apple 'NO' macro definition, it can cause build conflicts. Please, include this header before any Apple headers.
#endif
```
就是说苹果的'NO'宏定义会导致构建冲突, 所以为了解决冲突我们必须在包含所有苹果头文件前引入这个头文件, 所有我们一定要在pch文件中引入, 即添加:

```objc
#ifdef __cplusplus
#import <opencv2/opencv.hpp>
#endif
```

然后就可以愉快的使用OpenCV啦.

**PS:**OpenCV是C++的库, OC中使用C++的文件需要标识为'.mm'以便编译器区分.

[更多iOS资料](http://docs.opencv.org/3.0-last-rst/doc/tutorials/ios/table_of_content_ios/table_of_content_ios.html#table-of-content-ios)

[OpenCV iOS- Video Processing](http://docs.opencv.org/3.0-last-rst/doc/tutorials/ios/video_processing/video_processing.html#opencviosvideoprocessing)

