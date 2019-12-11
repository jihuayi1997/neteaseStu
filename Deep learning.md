# Deep Learning

## Neural Network

### 1. Summary

Supervised Learning

* Standard Neural Net
  * Read Estate : Home features...Price
  * Online Advertising : Ad,uer info...Click on ad?

* Convolutional Neural Network
  * Photo tagging : Image...Object(1,...,1000)
* Recurrent NN
  * Speech recognition : Audio...Text transcript
  * Machine translation : English...Chinese

* Hybrid NN
  * Autonomous driving : Image,Radar info...Position of other cars

Structured Data : databases of data

Unstructured Data : audio,raw audio,images,text

Scale drives deep learning progress

* Data
* Computation
* Algorithms : sigmod...rectified linear unit (ReLU)

### 2. Basic

#### Notation

​	![image-20191204202745676](TyporaPics/image-20191204202745676.png)

​	![image-20191204202803283](TyporaPics/image-20191204202803283.png)∈R<sup>n<sub>x</sub></sup>

​	y∈{0,1}

​	n=n<sub>x</sub>=64\*64\*3

​	(x,y)...a single training example

​	m=m<sub>train</sub>...number of training examples

​	m<sub>test</sub>...number of test examples

​	{(x<sup>(1)</sup>,y<sup>(1)</sup>),(x<sup>(2)</sup>,y<sup>(2)</sup>),...,(x<sup>(m)</sup>,y<sup>(m)</sup>)}...training set

​	X=[x<sup>(1)</sup>,x<sup>(2)</sup>,...,x<sup>(m)</sup>]...R<sup>n<sub>x</sub>*m</sup> matrix

​	Y=[y<sup>(1)</sup>,y<sup>(2)</sup>,...,y<sub>(m)</sub>]...R<sup>1*m</sup> matrix

#### logistic regression

![image-20191209111515223](TyporaPics/image-20191209111515223.png)

![image-20191209134050676](TyporaPics/image-20191209134050676.png)

#### Gradient Descent

![image-20191210173211289](TyporaPics/image-20191210173211289.png)

* w := w-α