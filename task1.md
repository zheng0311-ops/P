```python
import cv2
#图片读取
def read_picture():
    # 读取参数:图像参数
    image_path="C:\\Users\\Nonep\\Desktop\\armor_test.jpg"
    img=cv2.imread(image_path)
    return img
#灰度转换
def switch_gray():
    image_path="C:\\Users\\Nonep\\Desktop\\armor_test.jpg"
    img=cv2.imread(image_path)
    img_=img
    gray=cv2.cvtColor(img,cv2.COLOR_BGR2GRAY)
    #保留测试结果
    out_path1="C:\\Users\\Nonep\\Desktop\\rm\\task1\\gray_test2_pre.jpg"
    cv2.imwrite(out_path1,img_)
    out_path="C:\\Users\\Nonep\\Desktop\\rm\\task1\\gray_test2.jpg"
    cv2.imwrite(out_path,gray)
    return gray
#高斯模糊去噪
def blur():
    image_path1="C:\\Users\\Nonep\\Desktop\\shape_number_test.jpg"
    img1=cv2.imread(image_path1)
    img1_=img1
    #参数:(输入图像,(核的宽度,核的高度),在x方向上的标准差);宽度和高度必须是正数且为奇数
    blur_picture=cv2.GaussianBlur(img1,(11,11),0)
    out_path1="C:\\Users\\Nonep\\Desktop\\rm\\task1\\blur_test3_pre.jpg"
    cv2.imwrite(out_path1,img1_)
    out_path="C:\\Users\\Nonep\\Desktop\\rm\\task1\\blur_test3.jpg"
    cv2.imwrite(out_path,blur_picture)#参数:要保存到的路径、需要保存的照片
    return blur_picture
def equal():
    gray_path2="C:\\Users\\Nonep\\Desktop\\rm\\task1\\gray_test2.jpg"
    gray=cv2.imread(gray_path2)#读取灰度图
    equal_picture_pre=cv2.cvtColor(gray,cv2.COLOR_BGR2GRAY)#转成二维，即单通道灰色图
    equal_picture=cv2.equalizeHist(equal_picture_pre)#参数:单通道灰色图,作用:增强图像对比度
    #保存测试结果
    out_path1="C:\\Users\\Nonep\\Desktop\\rm\\task1\\equal_test4_pre.jpg"
    cv2.imwrite(out_path1,equal_picture_pre)
    out_path="C:\\Users\\Nonep\\Desktop\\rm\\task1\\equal_test4.jpg"
    cv2.imwrite(out_path,equal_picture)
    return equal_picture
```

