# triangle-using-aruco-markers
import cv2
import cv2.aruco as aruco
import numpy as np
flag1,flag2=0,0
cap=cv2.VideoCapture(0)
while(True):
    ret,frame=cap.read()
    img=cv2.cvtColor(frame, cv2.COLOR_BGR2GRAY)
    arucodict=aruco.Dictionary_get(aruco.DICT_6X6_250)
    parameters=aruco.DetectorParameters_create()
    corners,ids,rejectedImgPoints=aruco.detectMarkers(img,arucodict,parameters=parameters)
    imgcopy=frame.copy()
    if len(corners)==3:
        for x in ids:
            if x[0]==4:
                co1x,co1y=int(corners[0][0][0][0]+ corners[0][0][2][0])//2,int(corners[0][0][0][1]+ corners[0][0][2][1])//2
            if x[0]==6:
                co2x,co2y=int(corners[1][0][0][0]+ corners[1][0][2][0])//2,int(corners[1][0][0][1]+ corners[1][0][2][1])//2
            if x[0]==7:
                co3x,co3y=int(corners[2][0][0][0]+ corners[2][0][2][0])//2,int(corners[2][0][0][1]+ corners[2][0][2][1])//2
        cv2.line(imgcopy,(co1x,co1y),(co2x,co2y),(255,255,0),5)
        cv2.line(imgcopy,(co3x,co3y),(co2x,co2y),(255,255,0),5)
        cv2.line(imgcopy,(co1x,co1y),(co3x,co3y),(255,255,0),5)
        dist1=abs((abs(co2x-co1x))**2-(abs(co2y-co1y))**2)**0.5
        dist2=abs((abs(co1x-co3x))**2-(abs(co1y-co3y))**2)**0.5
        dist3=abs((abs(co3x-co2x))**2-(abs(co3y-co2y))**2)**0.5
        font = cv2.FONT_HERSHEY_SIMPLEX
        cv2.putText(imgcopy,"{}".format(int(dist1)),((co1x+co2x)//2,(co1y+co2y)//2),font,0.8,(255, 255, 0), 2,cv2.LINE_AA)
        cv2.putText(imgcopy,"{}".format(int(dist2)),((co1x+co3x)//2,(co1y+co3y)//2),font,0.8,(255, 255, 0), 2,cv2.LINE_AA)
        cv2.putText(imgcopy,"{}".format(int(dist3)),((co3x+co2x)//2,(co3y+co2y)//2),font,0.8,(255, 255, 0), 2,cv2.LINE_AA)
        
    cv2.imshow("image",imgcopy)
    if cv2.waitKey(1) & 0xFF == ord('q'):
        break
cap.release()
cv2.destroyAllWindows()
cap.release()
cv2.destroyAllWindows()
