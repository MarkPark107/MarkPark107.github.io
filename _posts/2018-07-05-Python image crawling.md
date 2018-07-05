---
layout: post
title: "파이썬 이미지 크롤링"
subtitle: "Python, Crawling Images"
categories: devlog
tags: python
---



> ## 파이썬, 이미지 크롤링

```python
import os
import requests
from bs4 import BeautifulSoup
from PIL import Image
MAXPIXELS = 65500

eq_url = '가져올 이미지의 referer가 될 주소'
headers = {
    'Referer' : eq_url
}
response = requests.get(eq_url)
if response.ok:
    soup = BeautifulSoup(response.text, 'lxml')
    img_rfs = soup.select('이미지를 구별할 태그')
    canvaswidth = 0
    imgheights = []
    imgfnames = []
    num = 0
    height = 0
    canvasheight = 0
    for img_rf in img_rfs:
        img_url = img_rf['src']
        img_data = requests.get(img_url, headers=headers).content
        img_fname = os.path.basename(img_url)
        with open(img_fname, 'wb') as img:
            img.write(img_data)
        with Image.open(img_fname) as im:
            canvaswidth = max(canvaswidth, im.width)
            if canvasheight + im.height > MAXPIXELS :
                #이전까지 읽던 데이터를 이미지파일로 생성
                with Image.new('RGB', (canvaswidth, canvasheight), (255, 255, 255)) as nim:
                    for imgfname, imgheight in zip(imgfnames, imgheights):
                        with Image.open(imgfname) as img:
                            nim.paste(img, box=(0,height))
                            height += imgheight
                    savefilename = 'all{}.jpg'.format(num)
                    nim.save(savefilename)
                    print('{} is created'.format(savefilename))
                    num += 1
                    canvaswidth = 0
                    canvasheight = im.height
                    del imgheights[:]
                    del imgfnames[:]
                    height = 0
                    imgheights.append(im.height)
                    imgfnames.append(img_fname)
            else :
                # 계속 읽기
                canvasheight += im.height
                imgheights.append(im.height)
                imgfnames.append(img_fname)
    if canvaswidth != 0:
        with Image.new('RGB', (canvaswidth, canvasheight), (255, 255, 255)) as nim:
            for imgfname, imgheight in zip(imgfnames, imgheights):
                with Image.open(imgfname) as img:
                    nim.paste(img, box=(0,height))
                    height += imgheight
            savefilename = 'all{}.jpg'.format(num)
            nim.save(savefilename)
            print('{} is created'.format(savefilename))
```

이번엔 여기까지 : )