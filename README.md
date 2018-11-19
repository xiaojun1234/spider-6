# spider-6
# Selenium
# 简单的Selenium(打开百度)全局变量
<pre>
from selenium import webdriver
browser = webdriver.Chrome(executable_path="./drivers/chromedriver.exe")   /*谷歌驱动环境*/
browser.get('http://www.baidu.com/')
</pre>
# 输入输出
## 输入
### 1.send keys with return 
<pre>
from selenium.webdriver.common.keys import Keys
kw = browser.find_element_by_id("kw")            /*搜索id为kw*/
kw.send_keys('Selenium',Keys.RETURN)
</pre>
### 2.send keys then click submit button
<pre>
kw = browser.find_element_by_id("kw")
su = browser.find_element_by_id("su")
kw.send_keys("Selenium")
su.click()
</pre>
### 3.send keys then submit form
<pre>
kw = browser.find_element_by_id("kw")
kw.send_keys("Selenium")
kw.submit()
</pre>
### 4.execute javascript
<pre>
browser.execute_script(
'''
var kw = document.getElementByld('kw');
var su = document.getElementByld('su');
kw.value = 'Selenium';
su.click();
'''
)
</pre>
## 输出
### 1.原始方式 parse page_source
<pre>
html = browser.page_source
results = parse_html(html)
</pre>
### 2. find & parse elements
<pre>
results = browser.find_elements_by_css_selector("#content_left.c-container")
for result in results:
    link = result.find_element_by_xpath(".//h3/a")    /*用于定位没有特殊标志的元素*/
    print(link.text)
</pre>
### 3.intercept & parse ajax
#### 得配合WebDriverWait和expected_conditions搭配使用
<pre>
from selenium.webdriver.common.by impoer By
from selenium.webdriver.support.ui import WebDriverWait
from selenium.webdriver.support import expected_conditions
WebDriverWait(browser,10).until(expected_conditions.presence_of_element_located((By.ID,"kw")))
使用别名简化：
from selenium.webdriver.common.by import By
from selenium.webdriver.support.ui import WebDriverWait as Wait
from selenium.webdriver.support import expected_conditions as Expect
Wait(browser,10).until(Expect.presence_of_element_located((By.ID,"kw")))
</pre>
# 实例
## 等待的条件：
### presence_of_element_located()
### text_to_be_present_in_element()
<pre>
from selenium import webdriver
from selenium.webdriver.common.by import By
from selenium.webdriver.support.ui import WebDriverWait as WAit
from selenium.webdriver.support import expected_conditions as Expect
browser = webdriver.Chrome(executable_path="./drivers/chromedriver.exe")
browser.get('http://www.tuniu.com/flight/intel/sha-sel')
Wait(browser,60).until(Expect.text_to_be_present_in_element((By.ID,"loadingStatus"),u"共搜索"))  /*搜索‘共搜索’标签*/
flight_items = browser.find_elements_by_class_name("flight-item")
for flight_item in flight_items:
    flight_price_row = flight_item.find_element_by_class_name("flight-price-row")
    print(flight_price_row.get_attribute("data-no"))
</pre>
