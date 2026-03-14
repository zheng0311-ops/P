```python
import cv2
import numpy
def chracter_determine():
    img1_path="C:\\Users\\Nonep\\Desktop\\rm\\task2\\e_d_red.jpg"
    img2_path="C:\\Users\\Nonep\\Desktop\\rm\\task2\\e_d_blue.jpg"
    img1=cv2.imread(img1_path)
    img2=cv2.imread(img2_path)
    gray1=cv2.cvtColor(img1,cv2.COLOR_BGR2GRAY)#红
    blur_picture1=cv2.GaussianBlur(gray1,(7,7),0)
    _1,binary1=cv2.threshold(blur_picture1,127,255,cv2.THRESH_BINARY)
    gray2=cv2.cvtColor(img2,cv2.COLOR_BGR2GRAY)#蓝
    blur_picture2=cv2.GaussianBlur(gray2,(7,7),0)
    _2,binary2=cv2.threshold(blur_picture2,127,255,cv2.THRESH_BINARY)
    contours1,hierArchy1=cv2.findContours(binary1,cv2.RETR_EXTERNAL,cv2.CHAIN_APPROX_SIMPLE)
    contours2,hierArchy2=cv2.findContours(binary2,cv2.RETR_EXTERNAL,cv2.CHAIN_APPROX_SIMPLE)
    #cv2.approxPolyDP()函数需要输入的三个参数:坐标数组,epsilon:逼近精度,是否闭合(True/False)
    #epsilon=0.02*图形周长
    print("---------红图----------")
    for c in contours1:
        #设置面积阈值起筛选作用
        if cv2.contourArea(c)>1000:
            #算圆度来判断是否是圆
            area=cv2.contourArea(c)
            perimeter=cv2.arcLength(c,True)
            roundness=4*numpy.pi*area/(perimeter**2)
            if(roundness>0.8):
                print("圆形")
            else:
                length=cv2.arcLength(c,True)
                epsilon=0.07*length#需要>=0.06
                approxCurve=cv2.approxPolyDP(c,epsilon,True)
                if(len(approxCurve)==4):
                    print("矩形")
                else:
                    print("其他")
    print("---------蓝图----------")
    for d in contours2:
        #设置面积阈值起筛选作用
        if cv2.contourArea(d)>1000:
            #算圆度来判断是否是圆
            area=cv2.contourArea(d)
            perimeter=cv2.arcLength(d,True)
            roundness=4*numpy.pi*area/(perimeter**2)
            #思路:每张测试图先打印查看各个图形的roundness从而控制范围,虽然鲁棒性不高
            if(roundness>0.86):
                    print("圆形")
            elif(0.84<roundness<0.86):
                    print("椭圆")
            else:
                length=cv2.arcLength(d,True)
                epsilon=0.07*length#需要>=0.06
                approxCurve=cv2.approxPolyDP(d,epsilon,True)
                if(len(approxCurve)==4):
                    print("矩形")
                else:
                    print("其他")
chracter_determine()       
```

