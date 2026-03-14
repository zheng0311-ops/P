```python
#识别0-9印刷体数字
import cv2
import numpy
#-------预处理------
def pre(img_path):
    img=cv2.imread(img_path)#读取图片
    #转灰度
    gray=cv2.cvtColor(img,cv2.COLOR_BGR2GRAY)
    #高斯模糊去噪
    #以目前测试的图片，因为两边都有发光条，可以将核大小调尽量大些，如果没有发光条，核大小(5,5)或者(11,11)即可
    blur=cv2.GaussianBlur(gray,(69,69),0)
    #转二值图
    _,binary=cv2.threshold(blur,127,255,cv2.THRESH_BINARY)
    return binary
def find(binary):
    contour,hierArchy=cv2.findContours(binary,cv2.RETR_EXTERNAL,cv2.CHAIN_APPROX_SIMPLE)
    return contour[0]
#-----0-9模板处理
contours=[]
img0_path="C:\\Users\\Nonep\\Pictures\\Screenshots\\0.jpg"
contour0=find(pre(img0_path))
contours.append(contour0)
img1_path="C:\\Users\\Nonep\\Pictures\\Screenshots\\1.jpg"
contour1=find(pre(img1_path))
contours.append(contour1)
img2_path="C:\\Users\\Nonep\\Pictures\\Screenshots\\2.png"
contour2=find(pre(img2_path))
contours.append(contour2)
img3_path="C:\\Users\\Nonep\\Pictures\\Screenshots\\3.png"
contour3=find(pre(img3_path))
contours.append(contour3)
img4_path="C:\\Users\\Nonep\\Pictures\\Screenshots\\4.png"
contour4=find(pre(img4_path))
contours.append(contour4)
img5_path="C:\\Users\\Nonep\\Pictures\\Screenshots\\5.png"
contour5=find(pre(img5_path))
contours.append(contour5)
img6_path="C:\\Users\\Nonep\\Pictures\\Screenshots\\6.png"
contour6=find(pre(img6_path))
contours.append(contour6)
img7_path="C:\\Users\\Nonep\\Pictures\\Screenshots\\7.png"
contour7=find(pre(img7_path))
contours.append(contour7)
img8_path="C:\\Users\\Nonep\\Pictures\\Screenshots\\8.png"
contour8=find(pre(img8_path))
contours.append(contour8)
img9_path="C:\\Users\\Nonep\\Pictures\\Screenshots\\9.png"
contour9=find(pre(img9_path))
contours.append(contour9)
#------待识别图片处理-------
img_path="C:\\Users\\Nonep\\Pictures\\Screenshots\\test5.jpg"
img=cv2.imread(img_path)
img1=img
outpath="C:\\Users\\Nonep\\Desktop\\rm\\task3\\test.jpg"
cv2.imwrite(outpath,img1)
contour=find(pre(img_path))
#--------模板轮廓匹配--------
#---matchShapes函数返回的是两个轮廓的差异分数，分数越小，二者越相似----
scores=[]
#---matchShapes函数的参数:第一个轮廓，第二个轮廓，比较方法(有3种)
for e in contours:
    score=cv2.matchShapes(contour,e,cv2.CONTOURS_MATCH_I2,0)
    scores.append(score)
res=scores.index(min(scores))
print(res)
```

