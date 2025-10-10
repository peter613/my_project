# my_project
# 前端

import tkinter as tk
root = tk.Tk()
root.title("選課系統")
root.geometry("600x400")
root.minsize(width=600,height=400) 

tk.Label(root, text="學號 ").grid(row=0, column=0, padx=10, pady=5, sticky="e")
entry_id = tk.Entry(root)
entry_id.grid(row=0, column=1, padx=10, pady=5)

tk.Label(root, text="密碼 ").grid(row=1, column=0, padx=10, pady=5, sticky="e")
entry_pwd = tk.Entry(root, show="*")
entry_pwd.grid(row=1, column=1, padx=10, pady=5)

tk.Label(root, text="選課").grid(row=2, column=0, padx=10, pady=10)
tk.Button(root, text="加選").grid(row=2, column=1, padx=5)
tk.Button(root, text="退選").grid(row=2, column=2, padx=5)

tk.Label(root, text="課程人數 (剩餘)").grid(row=3, column=0, padx=10)
tk.Button(root, text="體育/通識").grid(row=3, column=1, padx=5)
tk.Button(root, text="專業課程").grid(row=3, column=2, padx=5)

tk.Label(root, text="課表 / 成績").grid(row=4, column=0, padx=10, pady=20)
tk.Button(root, text="功課表").grid(row=4, column=1, padx=5)
tk.Button(root, text="學期成績").grid(row=4, column=2, padx=5)

tk.Label(root, text="畢業門檻").grid(row=5, column=0, padx=10)
tk.Button(root, text="畢業學分進度").grid(row=5, column=1, padx=5)
root.mainloop()


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
