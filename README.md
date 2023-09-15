![main](https://github.com/moschelin-app/client_mochelin_android/assets/124220561/ba1cdca6-b93e-4d61-bdaa-f56bea3d32c0)


# 당신이 찾는 맛집! 주변의 새로운 만남이 있는 곳, 모슐랭🍴

## 📌 Project Explanation
<div align="center">
   <img src="https://github.com/moschelin-app/client_mochelin_android/assets/108748094/99a28583-68cf-4bc1-9b01-49b79ac3143a" width=300 hight=300/>
   <img src="https://github.com/moschelin-app/client_mochelin_android/assets/108748094/75635ab2-086e-4c42-955b-fae1cb8d8056" width=300 hight=300/>
<br><br>

</div>
<div>
<h3> 
타인과 만나는 것에 원하면서도 두려워하는 젊은 세대들이 많고,<br> 
사회 전반적으로 외로움 수준이 최근 다시 높아진 상황을 보았을때 사람들과 쉽게 만날 수 있는 방법이 없을까?<br>
<br>
자신이 관심 있고 좋아하는 음식의 리뷰를 확인하면서 같이 음식을 먹을 수 있게 하는 모임을 만들 수 있다면, 
<br>타인과 만나는데 있어서 조금 더 쉽게 만날수 있게 도움을 주는 앱이 있으면 해서 만들었습니다.
</h3>
   </div>

---

<div align = "center">
  <h1>📚</h1>
  <img src="https://img.shields.io/badge/Python-3776AB?style=flat-square&logo=Python&logoColor=white"/> 
  <img src="https://img.shields.io/badge/Numpy-013243?style=flat-square&logo=Numpy&logoColor=white"/>
  <img src="https://img.shields.io/badge/Pandas-150458?style=flat-square&logo=Pandas&logoColor=white"/>
  <img src="https://img.shields.io/badge/Jupyter-F37626?style=flat-square&logo=jupyter&logoColor=white"/> 
  <br>
  <img src="https://img.shields.io/badge/Google Colab-F9AB00?style=flat-square&logo=googlecolab&logoColor=white"/>   
  <img src="https://img.shields.io/badge/Selenium-43B02A?style=flat-square&logo=selenium&logoColor=white"/>
  <img src="https://img.shields.io/badge/Json-000000?style=flat-square&logo=json&logoColor=white"/>
</div>



## 📌 Selenium Code

```Python 3
# 사용할 라이브러리
from selenium import webdriver
from selenium.webdriver.common.keys import Keys
from selenium.webdriver.common.by import By
from time import sleep
import pandas as pd
```

```Python 3
# 위도 경도 가져오기
# 검색된 주소에서 위도 경도를 가져옴
def get_location():
    current_url = driver.current_url

    lat_url = current_url.find("!3d") + 3
    lng_url = current_url.find("!4d") + 3
    lat = current_url[lat_url : lat_url+10]
    lng = current_url[lng_url : lng_url+11]

    return {'lat' : lat,
            'lng' : lng}
```


```Python 3
# 검색할 인천 리스트
keyword_list = [
  '서구',
  '남동구',
  '강화군',
  '계양구',
  '동구',
  '미추홀구',
  '부평구',
  '연수구',
  '옹진군',
  '중구',]

# 검색할 인천의 리스트를 돌림
for k in keyword_list:

    keyword = f'인천 {k} 맛집'

    # 검색어 입력
    search = driver.find_element(By.NAME, 'q')
    search.clear()
    search.send_keys(keyword)
    search.submit()
    driver.implicitly_wait(10)
    sleep(1)

    # 검색 시작 버튼 클릭
    search_button = driver.find_element(By.CSS_SELECTOR, 'div.pzfvzf')
    search_button.click()
    driver.implicitly_wait(10)
    sleep(5)

    # 스크롤 내리기
    scroll_div = driver.find_element(By.XPATH, '//*[@id="QA0Szd"]/div/div/div[1]/div[2]/div/div[1]/div/div/div[2]/div[1]')
    driver.implicitly_wait(10)
    sleep(2)

    # 검색결과가 많으면 계속 스크롤을 내림
    while 1:
        scroll_div.send_keys(Keys.END)
        driver.implicitly_wait(1)
        sleep(1)

        try:
            end_point = driver.find_element(By.XPATH, '//*[@id="QA0Szd"]/div/div/div[1]/div[2]/div/div[1]/div/div/div[2]/div[1]/div[243]/div/p/span/span')

            if end_point.text == '마지막 항목입니다.':
                break
        except:
            pass
        

    # 데이터를 넣을 csv 파일을 만듦
    place_columns = ['id', 'name', 'sub_name', 'addr', 'lat', 'lng']
    place_df = pd.DataFrame(columns = place_columns)

    review_columns = ['reivewId', 'content', 'image', 'star']
    review_df = pd.DataFrame(columns = review_columns)

    # 가게 정보 리스트 저장
    place_list = driver.find_elements(By.CSS_SELECTOR, 'div.Nv2PK.THOPZb.CpccDe > a.hfpxzc')
    driver.implicitly_wait(5)
    sleep(1)

    for i in range(len(place_list)):
        # 가게 클릭
        try:
            place_list[i].click()
            driver.implicitly_wait(10)
            sleep(2)
        except:
            print('해당 음식점 찾을 수 없음')
            continue

        # 위도 경도 가져오기
        location = get_location()
        
        # 가게 이름
        place_name = driver.find_element(By.XPATH, '//*[@id="QA0Szd"]/div/div/div[1]/div[3]/div/div[1]/div/div/div[2]/div[2]/div/div[1]/div[1]/h1').text
        
        # 가게 부가 정보
        place_sub_name = driver.find_element(By.XPATH, '//*[@id="QA0Szd"]/div/div/div[1]/div[3]/div/div[1]/div/div/div[2]/div[2]/div/div[1]/div[2]/div/div[2]/span/span/button').text

        # 가게 주소
        place_addr = driver.find_element(By.CSS_SELECTOR, 'div.Io6YTe.fontBodyMedium.kR99db').text
    

        # 가게정보를 DataFrame으로 만들어서 기존의 csv 파일과 합침
        place_sub_df = pd.DataFrame([[i, place_name, place_sub_name, place_addr, location['lat'], location['lng']]], columns=place_columns)

        place_df = pd.concat([place_df, place_sub_df], ignore_index=True)
        print(f"{i} 번째 가게 등록 --- 완료...")
        sleep(2)
        

        # 리뷰 탭 클릭
        review_button = driver.find_element(By.XPATH, '//*[@id="QA0Szd"]/div/div/div[1]/div[3]/div/div[1]/div/div/div[2]/div[3]/div/div/button[2]')
        review_button.click()
        driver.implicitly_wait(10)
        sleep(2)


        # 리뷰 글 전체
        review_div = driver.find_elements(By.CSS_SELECTOR, 'div.jftiEf.fontBodyMedium ')[:5]

        for review in review_div:
            try:
                # 별점 가져오기
                review_star = review.find_element(By.CSS_SELECTOR, 'span.kvMYJc').accessible_name[-2]
                
                # 리뷰 글
                review_content = review.find_element(By.CSS_SELECTOR, 'span.wiI7pd')
                review_content_str = '' if review_content == None else review_content.text

                # 이미지 URL
                review_image = review.find_element(By.CSS_SELECTOR, 'div.KtCyie > button')
                url = review_image.get_attribute('style')
                url = url[url.find('https://') : url.find('");')]
            except:
                pass

            # 리뷰 내용을 기존의 리뷰 내용과 함침
            reivew_sub_df = pd.DataFrame([[i, review_content_str, url, review_star]], columns=review_columns)

            review_df = pd.concat([review_df, reivew_sub_df], ignore_index=True)
            
        print(f"{place_name} 리뷰 등록 --- 완료...")
        sleep(2)


        # 합친 리뷰와 가게의 정보를 바로 저장시킴
        place_df.to_csv(f'C:/Users/Home/Documents/place{keyword}.csv', index=False, encoding='utf-8-sig')
        review_df.to_csv(f'C:/Users/Home/Documents/reivew{keyword}.csv', index=False, encoding='utf-8-sig')
```
