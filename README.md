# my_project
# 前端
#fojdsofjpo

# 後端
from selenium import webdriver
from selenium.webdriver.common.by import By
from selenium.webdriver.support.ui import Select
from selenium.webdriver.common.keys import Keys
import time
import pandas as pd

# id = int(input('請輸入帳號'))
# passwd = int(input('請輸入密碼'))

id = 12153217
passwd = 8100
scor_year = '113'
semester_num = '1'

options = webdriver.ChromeOptions()  
options.add_argument("--start-maximized") #全螢幕
driver = webdriver.Chrome(options=options)

url = 'https://web.sys.scu.edu.tw/default.asp'
url_score = 'https://web.sys.scu.edu.tw/score00.asp'
url_course = 'https://web.sys.scu.edu.tw/coursetbl00.asp'

driver.get(url)
driver.implicitly_wait(1)
time.sleep(1)

driver.switch_to.frame(2)
driver.implicitly_wait(1)
account = driver.find_element(By.NAME, "id")
password = driver.find_element(By.NAME, "passwd")
account.clear()
time.sleep(0.5)
account.send_keys(id)
time.sleep(1)

password.clear()
time.sleep(0.5)
password.send_keys(passwd)
time.sleep(1)
password.send_keys(Keys.ENTER)
time.sleep(1)

def score():
    driver.execute_script(f"window.open('{url_score}', '_blank');")
    time.sleep(0.5)
    driver.switch_to.window(driver.window_handles[1])#打開第二

    year = driver.find_element(By.NAME, "syear")
    year.clear()
    time.sleep(0.2)
    year.send_keys(scor_year)
    time.sleep(0.2)

    semester = driver.find_element(By.NAME, "smester")
    time.sleep(0.2)
    select = Select(semester)
    select.select_by_value(semester_num)
    time.sleep(0.2)

    enter = driver.find_element(By.XPATH, '/html/body/form/input[2]')
    enter.click()
    time.sleep(0.2)

    rank = driver.find_element(By.XPATH, '/html/body/table/caption/p').text
    return(rank)

# 有bug
def course():
    driver.execute_script(f"window.open('{url_course}', '_blank');")
    time.sleep(0.5)
    driver.switch_to.window(driver.window_handles[1])#打開第二

    year = driver.find_element(By.NAME, "procsyear")
    year.clear()
    time.sleep(0.2)
    year.send_keys(scor_year)
    time.sleep(0.2)

    semester = driver.find_element(By.NAME, "procterm")
    time.sleep(0.2)
    select = Select(semester)
    select.select_by_value(semester_num)
    time.sleep(0.2)

    enter = driver.find_element(By.XPATH, '/html/body/form/p/input[2]')
    enter.click()
    time.sleep(0.2)
    ths = driver.find_element(By.TAG_NAME, 'th')
    for _ in ths:
        return(_.text)