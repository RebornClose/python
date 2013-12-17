python
======

homework
    代码介绍：
    这个代码是实现从网上下载图片到本地的功能。代码是从网上找的，内容如下：
import urllib2
import urllib
import os
import urllister
# 这是来自http://diveintopython.org/html_processing/extracting_data.html#dialect.extract.urllib的一个分析html页面的类


imagepath=[]

#cd 函数用于判断路径是否正确，如正确则改变当前工作路径
def cd(ss):
    try:
        os.chdir(ss)
        print '改变工作目录为'+ss
        return 0
    except:
        print '输入图片保存路径有误，请重新输入'
        return 1
#addimagepath，该函数将图片路径添加到imagepath这个list中
def addimagepath(surl):
    if 'http://' in surl 
        imagepath.append(surl)
        print '找到图片：'+surl.split('/')[-1]+'图片地址为：'+surl
    else:
        surl=str_url+surl
        imagepath.append(surl)
        print '找到图片：'+surl.split('/')[-1]+'图片地址为：'+surl

#download images
def image_down(list_image):
    if not list_image:
        print "该页面没有任何图片"
    else:
        for image in list_image:
            try:
                urllib.urlretrieve(image,image.split('/')[-1]) #利用image.split('/')[-1]获得文件名
                print "来自"+image+"的图片保存成功！"
            except:
                print "来自"+image+"图片没有保存成功，继续保存下一张图片...."

print "请输入网页的url地址："
str_url=raw_input()

print "请输入图片保存地址，如果直接回车将默认保存到我的文档"

temp=1
while temp:
    str_save=raw_input()
    if not str_save:
        str_save='E:/Fei_Doc'
    temp=cd(str_save)
    


try:
    sock=urllib2.urlopen(str_url)
    print "页面连接成功！开始获取图片地址……"
except:
    print "sorry，输入的地址有误或页面无法连接，程序将自动退出"

parser=urllister.URLLister()
parser.feed(sock.read())
sock.close()
parser.close()
for url in parser.urls:
    addimagepath(url)

#调用图片下载函数
image_down(imagepath)

#程序结束

程序是要用到上节课讲到的urllib（2）的库以及其他几个库如os库，这是对目录操作的库。操作网址定为http://diveintopython.org/html_processing/extracting_data.html


对代码的理解和体会有加进更新中，复制过来：
在diveintopython网站上，http://diveintopython.org/html_processing/extracting_data.html可以找到一些html处理的例子，比如这个类可以用来获取html页面中的href标签内容。
from sgmllib import SGMLParser

class URLLister(SGMLParser):
    def reset(self):                              
        SGMLParser.reset(self)
        self.urls = []

    def start_a(self, attrs):                     
        href = [v for k, v in attrs if k=='href']  
        if href:
            self.urls.extend(href)



将这个文件href的地方都改成src，start_a改成start_img
即：
from sgmllib import SGMLParser
class URLLister(SGMLParser):
    def reset(self):                              
        SGMLParser.reset(self)
        self.urls = []
    def start_img(self, attrs):                     
        src = [v for k, v in attrs if k=='src'] 
        if src:
            self.urls.extend(src)
保存代码内容为urllister.py文件，放在python安装目录即可，这样就可以用来分析网页的图片地址了。


尽管这个程序能基本上解决问题，不过我发现有一些不足：
1、如果页面的img标签后面没有直接跟src属性，比如复杂一点的img代码：
 <img border="0" onload="
function onload(event) {
javascript:
    if (this.width > screen.width - 333) {
        this.width = screen.width - 333;
    }
}
" src="http://bbs.263.net/forumData/119/2860134_1.jpg" alt="按此在新窗口浏览图片" />

那么urllister便无法识别了。不过这个问题解决起来比较容易，直接对html代码每行代码进行分析，利用split('src')，可以得到所有src标签的内容，然后根据后缀是否为jpg，gif等得到图片文件地址。
        

2、上面的程序只是对地址为http开始的图片以及当前url下面目录的图片进行处理，如果src里面的内容以“../images“ 或者 “/“开头，则需另外处理。



