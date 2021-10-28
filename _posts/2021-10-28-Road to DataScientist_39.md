---
layout: post
title:  "Road to Datascientist - 35. Computer Vision - video splicing"
date:   2021-10-28
use_math: true
comments: true
categories: DataScience 
tags: DataScience OpenCV
---
# Computer Vision

---

# 1. Computer Vision

* 이번 포스팅 부터는 **Computer Vision**에 대해 포스팅 하도록 하겠습니다.
* 주로 **Open CV**를 사용할 예정이며 `jupyter notebook` 혹은 `Pycharm`환경에서 작업하려고 합니다.
* **Coumputer Vision**이란 사진이나 동영상과 같은 시각적 데이터를 해석하고 이해하는 인공지능 분야입니다.

---
# 2. Open CV

* **Open CV**는 **Computer Vision**분야에서 자주 쓰이는 툴 입니다.
* **Open CV**는 학습을 시도해 weight를 구하는 것이 아닌 **pretrained 된 model**을 사용하여 문제를 해결 할 것 입니다.
* python에서 Open CV의 라이브러리를 불러오기 위해서는 `import cv2`를 이용하면 됩니다.
* 첫 초기버전 부터 cv라는 이름의 라이브러리를 사용하다가 버전이 진행되면서 cv2를 사용하게 되었습니다.

# 2.1 Open CV 이용

* 이번 포스팅 부터는 실제 작성한 코드를 이용하여 **Open CV**를 이용하는 법을 연습하도록 하겠습니다.
* 사용된 코드는 아래 주소에 공개되어 있습니다.

> <https://github.com/nanoteyep/Python_practice/tree/master/OpenCV>

---
# 3. Video splicing

* 이번에 연습한 코드는 video1 과 video2를 하나의 동영상으로 연결하는 코드 입니다.

```python
# Open Video
cap1 = cv2.VideoCapture('video1.mp4')
cap2 = cv2.VideoCapture('video2.mp4')

if not cap1.isOpened() or not cap2.isOpened():
    print("The video is not opend!!")
    sys.exit()
```

* 각각의 video를 불러오는 코드이며 isOpened()함수를 사용하여 비디오를 정상적으로 불러왔는지 확인합니다.

```python
# assume that the two video has same fps, width, height
frame_cnt1 = round(cap1.get(cv2.CAP_PROP_FRAME_COUNT))
frame_cnt2 = round(cap2.get(cv2.CAP_PROP_FRAME_COUNT))
fps = cap1.get(cv2.CAP_PROP_FPS)
effect_frames = int(fps * 2)

print('frame_cnt1:', frame_cnt1)
print('frame_cnt2:', frame_cnt2)
print('fps:', fps)


delay = int(1000 / fps)

w = round(cap1.get(cv2.CAP_PROP_FRAME_WIDTH))
h = round(cap1.get(cv2.CAP_PROP_FRAME_HEIGHT))
fourcc = cv2.VideoWriter_fourcc(*'DIVX')
```

* 각 영상의 정보를 변수에 저장하며 frame 수, fps를 확인합니다.

* 위 코드에서는 두 영상이 같은 fps, width, height를 가지고 있다고 사전에 알고 있었기에 굳이 위 변수들을 두개로 나누지 않고 하나의 변수에 저장하였습니다.

```python
# video out
out = cv2.VideoWriter('output.avi', fourcc, fps, (w, h))
```

* out이라는 output객채를 만들어 줍니다.
* 이 코드의 목적은 out객체를 video1 + 변환부분 + video2로 구성된 하나의 video로 만드는 것 입니다.

```python
# copy video 1
for i in range(frame_cnt1 - effect_frames):
    ret1, frame1 = cap1.read()
    
    if not ret1:
        print('frame read error!')
        sys.exit()
        
    out.write(frame1)
    print('.', end='')
    
    cv2.imshow('output', frame1)
    cv2.waitKey(delay)
```

* video1을 우선 out객체에 넣는 과정입니다.
* frame을 계산해보면 0 부터 video1의 frame에서 effect frames를 뺀, 즉 변환부분의 frame을 뺀 부분까지만 저장하는 것 입니다.

```python
# combine back part of video1 and front part of video2    
for i in range(effect_frames):
    ret1, frame1 = cap1.read()
    ret2, frame2 = cap2.read()
    
    if not ret1 or not ret2:
        print('frame read error!')
        sys.exit()
    
# change video1 to video2 sequencially in a axis of width
    dx = int(w / effect_frames) * i
    
    frame = np.zeros((h, w, 3), dtype=np.uint8)
    frame[:, 0:dx, :] = frame2[:, 0:dx, :]
    frame[:, dx:w, :] = frame1[:, dx:w, :]
    
    out.write(frame)
    print('.', end='')
    
    cv2.imshow('output', frame)
    cv2.waitKey(delay)
```

* 변환부분 입니다.
* 중간 부분이 어떤 방식으로 변화하는지를 설정한 코드이며 이번 코드에서는 video1이 x축을 따라 순서대로 video2로 되는 연출이 사용되었습니다.

```python
# copy video2
for i in range(effect_frames, frame_cnt2):
    ret2, frame2 = cap2.read()
    
    if not ret2:
        print("frame read error!")
        sys.exit()
        
    out.write(frame2)
    print('.', end='')
    
    cv2.imshow('output', frame2)
    cv2.waitKey(delay)
    
print('\noutput file is successfully generated!')
```
* 마지막으로 out 객체에 video2를 넣고 작업을 마칩니다.

```python
cap1.release()
cap2.release()
out.release()
cv2.destroyAllWindows
```

* open했던 파일들은 release()함수로 다시 닫아 줄 수 있습니다.

---
# 마치며
**Open CV**를 이용하면 실제 프로그램이 동작하는 도중 새로운 창이 생성되며 영상을 확인 할 수 있을 것 입니다. 이렇게 Open CV는 새로운 window를 생성하여 작업 결과를 확인 할 수도 있으며 실제로 write()함수를 이용하여 파일로 만들어 저장할 수도 있습니다. 당분간은 **Computer Vision**분야에서 자주 사용하는 툴인 **Open CV**를 코드위주로 다뤄볼 예정입니다.