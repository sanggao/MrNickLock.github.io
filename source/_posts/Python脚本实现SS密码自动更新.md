---
title: Python脚本实现SS密码自动更新
date: 2017-07-07 21:18:36
tags:
	- python
---

`mycmd`下新建`ss.bat`

```shell
#更新密码
ssr/ss.py
#启动 SS
ssr/ss.exe

```

CodeBak

File:`ss.py`

```python
import zbar
from PIL import Image
import urllib
import cStringIO
import base64
import sys,os


URL = ('http://freess.org/images/servers/jp03.png')
# create a reader
scanner = zbar.ImageScanner()
# configure the reader
scanner.parse_config('enable')
# obtain image data
imgfile = cStringIO.StringIO(urllib.urlopen(URL).read())
pil = Image.open(imgfile).convert('L')

width, height = pil.size
raw = pil.tostring()
# wrap image data
image = zbar.Image(width, height, 'Y800', raw)
# scan the image for barcodes
scanner.scan(image)
# extract results
for symbol in image:
    # do something useful with results
    # print 'decoded', symbol.type, 'symbol', '"%s"' % symbol.data
    pwd = base64.b64decode(symbol.data[5:])[12:20]
    # with open('p1.txt', 'w') as f:
    #     f.write(pwd)

f=open('gui-config.json','r+')
flist=f.readlines()
#替换第26行
flist[25]='          \"password\" : \"'+pwd+'\",\n'
f=open('gui-config.json','w+')
f.writelines(flist)


# clean up
del(image)
```