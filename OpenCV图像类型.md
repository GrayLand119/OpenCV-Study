# OpenCV图像类型

Mat侧重于计算, OpenCV对Mat类型计算进行了优化.

CvMat / IplImage侧重于图像, OpenCV对图像操作(缩放/单通道提取/图像阈值操作等)进行了优化.

## 1. Mat

### 1.1 基本介绍

Mat基于C++的接口, 相比于IplImage它**不需要手动开辟空间和释放.**大多数OpenCV函数仍会手动开辟空间, 当传递已经存在的Mat对象时, **开辟好的矩阵空间会被重用**.

Mat主要分两部分, 一部分是矩阵头(矩阵尺寸、存储方法、存储地址信息), 另一部分是指向存储所有像素的矩阵指针. Mat使用ARC内存管理机制, 每个Mat对象有自己的信息头, 但共享一个矩阵.

* 而拷贝构造函数则**只拷贝信息头和矩阵指针**, 而不拷贝矩阵.

拷贝一个Mat对象需要用到 **clone()** 或 **copyTo()**:

```cpp
...
Mat B = A.clone()
A.copyTo(B)
...
```
### 1.2 显式地创建一个Mat对象

首先介绍一下常用的宏**CV_8UC3**, 类似的还有:

```cpp
#define CV_8UC1 CV_MAKETYPE(CV_8U,1)
#define CV_8UC2 CV_MAKETYPE(CV_8U,2)
#define CV_8UC3 CV_MAKETYPE(CV_8U,3)
#define CV_8UC4 CV_MAKETYPE(CV_8U,4)
#define CV_8UC(n) CV_MAKETYPE(CV_8U,(n))
```

对于二维多通道图像，首先要定义其尺寸，即行数和列数。
然后，需要指定存储元素的数据类型以及每个矩阵点的通道数。为此，依据下面的规则有多种定义

> CV_[The number of bits per item][Signed or Unsigned][Type Prefix]C[The channel number]

比如 CV_8UC3 表示使用8位的 unsigned char 型，每个像素由三个元素组成三通道。预先定义的通道数可以多达四个。 Scalar 是个short型vector。指定这个能够使用指定的定制化值来初始化矩阵。当然，如果你需要更多通道数，你可以使用大写的宏并把通道数放在小括号中，如下所示:

```cpp
int sz[3] = {2,2,2}; 
Mat L(3,sz, CV_8UC(1), Scalar::all(0));
```

1.**Mat()构造函数**

```cpp
Mat M(2,2, CV_8UC3, Scalar(0,0,255)); 
cout << "M = " << endl << " " << M << endl << endl; 
```
![image1](http://www.opencv.org.cn/opencvdoc/2.3.2/html/_images/MatBasicContainerOut1.png)

2.**使用mtx()函数为已存在IplImage创建信息头**

```cpp
IplImage* img = cvLoadImage("greatwave.png", 1);
Mat mtx(img); // convert IplImage* -> Mat
```

3.**使用Create()函数**

这个方法不能为矩阵设置初始值, 它只是在改变尺寸时重新为矩阵数据开辟内存

```cpp
M.create(4,4, CV_8UC(2));
cout << "M = "<< endl << " "  << M << endl << endl;
```

![imagecreate](http://www.opencv.org.cn/opencvdoc/2.3.2/html/_images/MatBasicContainerOut2.png)

4.**MATLAB形式的初始化方式： zeros(), ones(), :eyes() 。使用以下方式指定尺寸和数据类型**

```cpp
    Mat E = Mat::eye(4, 4, CV_64F);    
    cout << "E = " << endl << " " << E << endl << endl;
    
    Mat O = Mat::ones(2, 2, CV_32F);    
    cout << "O = " << endl << " " << O << endl << endl;

    Mat Z = Mat::zeros(3,3, CV_8UC1);
    cout << "Z = " << endl << " " << Z << endl << endl;
```

![imagematlab](http://www.opencv.org.cn/opencvdoc/2.3.2/html/_images/MatBasicContainerOut3.png)

5.**对于小矩阵你可以用逗号分隔的初始化函数**

```cpp
    Mat RowClone = C.row(1).clone();
    cout << "RowClone = " << endl << " " << RowClone << endl << endl;
```

![image5](http://www.opencv.org.cn/opencvdoc/2.3.2/html/_images/MatBasicContainerOut6.png)

6.**使用 clone() 或者 copyTo() 为一个存在的 Mat 对象创建一个新的信息头**

```cpp
    Mat RowClone = C.row(1).clone();
    cout << "RowClone = " << endl << " " << RowClone << endl << endl;
```

![image6](http://www.opencv.org.cn/opencvdoc/2.3.2/html/_images/MatBasicContainerOut7.png)

### 1.3 格式化打印

调用函数 randu() 来对一个矩阵使用随机数填充，需要指定随机数的上界和下界:

```cpp
    Mat R = Mat(3, 2, CV_8UC3);
    randu(R, Scalar::all(0), Scalar::all(255));
```

1.**默认方式:**cout << R

![img1](http://www.opencv.org.cn/opencvdoc/2.3.2/html/_images/MatBasicContainerOut8.png)

2.**Python:**cout << format(R,"python") 

![img2](http://www.opencv.org.cn/opencvdoc/2.3.2/html/_images/MatBasicContainerOut16.png)

3.**CSV:**cout << format(R, "csv")

![img3](http://www.opencv.org.cn/opencvdoc/2.3.2/html/_images/MatBasicContainerOut10.png)

4.**Numpy:**cout << format(R, "numpy")

![img4](http://www.opencv.org.cn/opencvdoc/2.3.2/html/_images/MatBasicContainerOut9.png)

5.**C语言:**cout << format(R, "C")

![img5](http://www.opencv.org.cn/opencvdoc/2.3.2/html/_images/MatBasicContainerOut11.png)




[详细资料](http://www.opencv.org.cn/opencvdoc/2.3.2/html/doc/tutorials/core/mat%20-%20the%20basic%20image%20container/mat%20-%20the%20basic%20image%20container.html#matthebasicimagecontainer)

## CvMat

官网介绍说CvMat已经被淘汰, 建议使用Mat替代

[CvMat结构](http://www.opencv.org.cn/opencvdoc/2.3.2/html/modules/core/doc/old_basic_structures.html?highlight=cvmat#CvMat)


## IplImage

IplImage全称是Intel Procesing Library Image, 顾名思义, 是Intel系统原生格式.
OpenCV可能只支持IplImage的子集.

[IplImage结构](http://www.opencv.org.cn/opencvdoc/2.3.2/html/modules/core/doc/old_basic_structures.html?highlight=iplimage#IplImage)

## 几种格式之间的转换

1.Mat -> IplImage

```cpp
IplImage pImg= IplImage(imgMat);
```

2.Mat -> CvMat

```cpp
CvMat cvMat = imgMat;
```

3.IplImage -> Mat

```cpp
IplImage* img = cvLoadImage("greatwave.png", 1);
Mat mtx(img); // convert IplImage* -> Mat
```

4.IplImage -> CvMat

```cpp
CvMat cvMat = cvGetImage(matI,img);
```

5.CvMat -> Mat

```cpp
Mat::Mat(const CvMat* m, bool copyData=false); // 可以选择是否复制数据```

6.CvMat -> IplImage

```cpp
// Mathod 1:
CvMat mathdr, *mat = cvGetMat( img, &mathdr );

// Mathod 2:
CvMat *mat = cvCreateMat(img->height, img->width, CV_64FC3);
cvConvert(img, mat);
```

7.IplImage * -> Byte *

```cpp
BYTE *data = img->imageData;
```

8.BYTE * -> IplImage *

```cpp
IplImage *img = cvCreateImageHeader(cvSize(width,height), depth, channels);
cvSetData(img, data, step);// step指定图像每行占的字节数
```


