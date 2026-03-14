```python
import cv2
import numpy
def cut_color():
#基于HSV颜色空间实现红色目标分割
    img1_path="C:\\Users\\Nonep\\Desktop\\color_test.jpg"
    img1=cv2.imread(img1_path)#读取图片
    hsv_img1=cv2.cvtColor(img1,cv2.COLOR_BGR2HSV)#BGR转为HSV
    #红色阈值分割
    #红色的H值分布在[0,10]、[160,180]
    lower_red=numpy.array([0,100,100])#下界
    upper_red=numpy.array([10,255,255])#上界
    lower_blue=numpy.array([90,100,100])#下界
    upper_blue=numpy.array([120,255,255])#上界
    #函数功能---颜色阈值分割
    mask1=cv2.inRange(hsv_img1,lower_red,upper_red)#参数：红色阈值下界、红色阈值上界
    mask2=cv2.inRange(hsv_img1,lower_blue,upper_blue)#参数:蓝色阈值下界、蓝色阈值上界
    #保存测试结果
    out_path="C:\\Users\\Nonep\\Desktop\\rm\\task2\\hsv_img1.jpg"
    cv2.imwrite(out_path,hsv_img1)
    out_path1="C:\\Users\\Nonep\\Desktop\\rm\\task2\\cut_red.jpg"
    cv2.imwrite(out_path1,mask1)
    out_path2="C:\\Users\\Nonep\\Desktop\\rm\\task2\\cut_blue.jpg"
    cv2.imwrite(out_path2,mask2)
    return mask1,mask2
#形态学的侵蚀和膨胀
def erode_dilate():
    img1_path="C:\\Users\\Nonep\\Desktop\\rm\\task2\\cut_red.jpg"
    img1=cv2.imread(img1_path)
    img2_path="C:\\Users\\Nonep\\Desktop\\rm\\task2\\cut_blue.jpg"
    img2=cv2.imread(img2_path)
    kernel=numpy.ones((4,4),numpy.uint8)#核大小为4*4,里面全是1,0会侵蚀
    eroded_img1=cv2.erode(img1,kernel,iterations=1)#iterations表示腐蚀次数
    dilated_img1=cv2.dilate(eroded_img1,kernel,iterations=1)
    eroded_img2=cv2.erode(img2,kernel,iterations=1)
    dilated_img2=cv2.dilate(eroded_img2,kernel,iterations=1)
    #保存测试结果
    out_path1="C:\\Users\\Nonep\\Desktop\\rm\\task2\\e_d_red.jpg"
    cv2.imwrite(out_path1,dilated_img1)
    out_path2="C:\\Users\\Nonep\\Desktop\\rm\\task2\\e_d_blue.jpg"
    cv2.imwrite(out_path2,dilated_img2)
    return dilated_img1,dilated_img2
#---------求质心----------
def getcenter(contour):
    M=cv2.moments(contour)#获得轮廓的矩
    if M["m00"]==0:#"m00"表示轮廓的面积
        return None
    #x=m10/m00、y=m01/m00
    x=int(M["m10"]/M["m00"])
    y=int(M["m10"]/M["m00"])
    return(x,y)
#筛选有效目标轮廓并标注其坐标面积信息
def choose_sign():
    img1,img2=erode_dilate()#先红后蓝
    gray1=cv2.cvtColor(img1,cv2.COLOR_BGR2GRAY)
    gray2=cv2.cvtColor(img2,cv2.COLOR_BGR2GRAY)
    #输入三大参数:二值化图像(大多是单通道的灰色图),轮廓检索算法的模式,轮廓近似方法
    #轮廓检索算法的模式中cv2.RETR_EXTERNAL函数:仅检索最外层的轮廓，它为所有轮廓设置hierArchy[i][2]=hierArchy[i][3]=-1
    #轮廓近似方法中cv2.CHAIN_APPROX_SIMPLE函数:仅保留端点，矩形4个，圆4个
    #输出的二大元素:contours:检测到的轮廓,每个轮廓都存储为点向量;hierArchy:可选的输出向量
    #----------二值化--------
    _1,binary1=cv2.threshold(gray1,127,255,cv2.THRESH_BINARY)
    _2,binary2=cv2.threshold(gray2,127,255,cv2.THRESH_BINARY)
    #----------获取轮廓------
    contours1,hierArchy1=cv2.findContours(binary1,cv2.RETR_EXTERNAL,cv2.CHAIN_APPROX_SIMPLE)
    #print(contours1)#输出每个轮廓的数组,里面存储多个点向量,太多坐标了
    #print(hierArchy1)结果:[[[ 1 -1 -1 -1][-1  0 -1 -1]]]
    contours2,hierArchy2=cv2.findContours(binary2,cv2.RETR_EXTERNAL,cv2.CHAIN_APPROX_SIMPLE)
    print("--------红---------")
    list1=[]
    for a in contours1:
        area=cv2.contourArea(a)
        if area<100:
            continue
        center=getcenter(a)
        if center:
            x,y=center
            print(f"质心坐标:({x},{y})")
            list1.append(area)
    print("---------蓝----------")
    list2=[]
    for b in contours2:
        area=cv2.contourArea(b)
        if area<100:#设置阈值排除噪点小的面积
            continue
        center=getcenter(b)
        if center:
            x,y=center
            print(f"质心坐标:({x},{y})")
            list2.append(area)
    #print(hierArchy2)#结果:[[[ 1 -1 -1 -1][ 2  0 -1 -1][-1  1 -1 -1]]]
    print(list1)#输出结果:[10125.5, 4907.0]
    print(list2)#输出结果:[2878.0, 7845.0, 6219.0]
choose_sign()
#质心坐标:
#-------红------
#(101,101)    (100,100)
#-------蓝------
#(301,301)    (301,301)     (300,300)
```

