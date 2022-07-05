---
layout: single
title : "Matplotlib을 이용한 데이터 분석-(1)"
categories: Data_Analysis
tag: Data
---

[ 2021년 코로나 일일 확진자 수와 인천공항 항공 데이터를 활용한 데이터 분석 ]  
===


## < 데이터를 활용하게 된 계기 > 
코로나로 인한 피로가 심해지기 시작한 2021년에도 해외 방문을 하는 사람의 수가 궁금하게 되어 관련 데이터들을 찾고 활용하게 되었습니다.  


## < 가설 >
코로나 확진자 수와 국제 항공기 이용자 수는 반비례 할 것이다.  
  

## < 작성한 코드 >
```python
import csv
import matplotlib.pyplot as plt

# 데이터가 저장되어 있는 csv형식의 파일을 불러온다.
Airline_file = open('2021_IncheonAirport_Airline.csv') 
Original_Airline_Data = csv.reader(Airline_file) # 인천공항 항공 데이터

Corona_Confirmed_File = open( 'Corona_Confirmed_2020_2022.csv' )
Corona_Data = csv.reader( Corona_Confirmed_File ) # 코로나 일일 확진자 수 데이터


''' < 데이타 변수들 >'''
Oversea_Monthly_Users = [0] * 12 # 월별 국제 항공기 사용 인원들 수를 저장한 배열
Corona_Case_2021 = [0] * 12 # 월별 코로나 확진자 수를 저장한 배열



# 데이터 값들 중 맨 위 목차를 넘기기 위함
next(Original_Airline_Data)

next(Corona_Data) 
next(Corona_Data)


# 출발 및 여객용 항공기에 초점을 두고 월별로 데이터를 분류시킴.
for row in Original_Airline_Data :
    
    if ( row[6] == '출발' and row[8] == '여객' ) or ( row[7] == '출발' and row[8] == '여객' ) :
        
        if row[4] == '국제' :
            if row[0][:3] == 'Jan' :
                Oversea_Monthly_Users[0] += ( int(row[9]) + int(row[10]) + int(row[11]) )
            elif row[0][:3] == 'Feb' :
                Oversea_Monthly_Users[1] += ( int(row[9]) + int(row[10]) + int(row[11]) )
            elif row[0][:3] == 'Mar' :
                Oversea_Monthly_Users[2] += ( int(row[9]) + int(row[10]) + int(row[11]) )
            elif row[0][:3] == 'Apr' :
                Oversea_Monthly_Users[3] += ( int(row[9]) + int(row[10]) + int(row[11]) )
            elif row[0][:3] == 'May' :
                Oversea_Monthly_Users[4] += ( int(row[9]) + int(row[10]) + int(row[11]) )
            elif row[0][:3] == 'Jun' :
                Oversea_Monthly_Users[5] += ( int(row[9]) + int(row[10]) + int(row[11]) )
            elif row[0][:3] == 'Jul' :
                Oversea_Monthly_Users[6] += ( int(row[9]) + int(row[10]) + int(row[11]) )
            elif row[0][:3] == 'Aug' :
                Oversea_Monthly_Users[7] += ( int(row[9]) + int(row[10]) + int(row[11]) )
            elif row[0][:3] == 'Sep' :
                Oversea_Monthly_Users[8] += ( int(row[9]) + int(row[10]) + int(row[11]) )
            elif row[0][:3] == 'Oct' :
                Oversea_Monthly_Users[9] += ( int(row[9]) + int(row[10]) + int(row[11]) )
            elif row[0][:3] == 'Nov' :
                Oversea_Monthly_Users[10] += ( int(row[9]) + int(row[10]) + int(row[11]) )
            elif row[0][:3] == 'Dec' :
                Oversea_Monthly_Users[11] += ( int(row[9]) + int(row[10]) + int(row[11]) )

                
# 일별 코로나 확진자 데이터를 월별 총확진자수로 정리
for row in Corona_Data :
    if ( row[0].split('-')[0] ) == '2021' :
        Corona_Case_2021[ int( row[0].split('-')[1] ) - 1 ] += int( row[1].replace(',','') )


        
# 필요한 데이터를 분류한 두 데이터들을 그래프화
plt.rc('font', family = 'Malgun Gothic')
plt.title('2021년 코로나 확진자 수와 국제 항공기 이용자 수')
plt.plot(range(0,12,1), Oversea_Monthly_Users, color = 'Blue' )
plt.xticks( ticks=[0,1,2,3,4,5,6,7,8,9,10,11],
           labels=['1월', '2월', '3월', '4월', '5월', '6월', '7월', '8월', '9월', '10월', '11월', '12월'] )
plt.plot( range(0, 12), Corona_Case_2021, color = 'Red' )
plt.show()

```
![2022-07-04-Data_Analysis.PNG](/images/2022-07-04-Data_Analysis.PNG)


## < 그래프를 통한 사실과 추측 >
- 코로나 확진자 수와 국제 항공기 이용자 수는 반비례하지 않았다.
- 코로나에 대한 백신이 보급되고 치사율이 줄어들기 시작하면서 코로나에 대한 위협이 점점 감소하여 해외 방문자 수가 증가하게 된 것 같다.  


## < 진행과정 중 어려웠던 점 >
1. 내가 구상하고 필요한 데이터가 인터넷상에 존재하지 않을 수도 있다.
2. 내가 원한 데이터를 구했다 하더라도 원하는 데이터만 있는 것이 아닌 다양한 종류의 데이터가 섞여 있었다. 이 중 원하는 데이터를 추출하는데 큰 불편함을 느꼈다.  


## < 다음 데이터 분석 시 개선 할 점 >
1. pandas와 numpy를 이용하여 코드를 좀 더 간결하게 작성해볼 것.  


## < 데이터 출처 >
[상반기 항공자료](https://odp.airport.kr/apiPortal/metadata)  
[하반기 항공자료](https://www.data.go.kr/data/15062056/fileData.do)  
[코로나](http://ncov.mohw.go.kr/)