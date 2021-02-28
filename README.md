
# 1. docker run
```
$ xhost +; sudo docker run --gpus all -it --rm -v /mnt/docker/tlt-experiments:/workspace/tlt-experiments -v /tmp/.X11-unix:/tmp/.X11-unix --device /dev/video0:/dev/video0:mwr -e DISPLAY=$DISPLAY -p 8888:8888 nvcr.io/nvidia/tlt-streamanalytics:v3.0-dp-py3 /bin/bash
```

# 2. run webcam test
```
----- in the container -----
root@52b289cd78c4:/workspace# export QT_X11_NO_MITSHM=1
root@52b289cd78c4:/workspace# cat tlt-experiments/sample/test.py 
import cv2
camera_width = 1280
camera_height = 960
vidfps = 30

cam = cv2.VideoCapture(0)
cam.set(cv2.CAP_PROP_FPS, vidfps)
cam.set(cv2.CAP_PROP_FRAME_WIDTH, camera_width)
cam.set(cv2.CAP_PROP_FRAME_HEIGHT, camera_height)
cv2.namedWindow("USB Camera", cv2.WINDOW_AUTOSIZE)

while True:
    ret, color_image = cam.read()
    if not ret:
        continue
    cv2.imshow('USB Camera', color_image)
    if cv2.waitKey(1)&0xFF == ord('q'):
        break
root@52b289cd78c4:/workspace# python tlt-experiments/sample/test.py 

```
