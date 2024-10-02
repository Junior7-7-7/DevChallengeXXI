# DevChallengeXXI

devchallenge_test/
│
├── test_email_visibility.py
├── test_count_judges.py
├── test_no_mobile_partners.py
├── README.md
└── requirements.txt

1.

test_email_visibility.py
from selenium import webdriver
from selenium.webdriver.common.by import By
from webdriver_manager.chrome import ChromeDriverManager
import time
def test_contact_email_visible():
driver = webdriver.Chrome(ChromeDriverManager().install())
try:
driver.get("https://www.devchallenge.it")
# Navigate to About page
about_menu = driver.find_element(By.LINK_TEXT, "DEV Challenge")
about_menu.click()
time.sleep(2)
about_page_link = driver.find_element(By.LINK_TEXT, "About")
about_page_link.click()
time.sleep(2)
# Scroll to the bottom
driver.execute_script("window.scrollTo(0, document.body.scrollHeight);")
time.sleep(2)
# Verify email is displayed
email_element = driver.find_element(By.XPATH, "//body[contains(text(), 'hello@devchallenge.it')]")
assert email_element.is_displayed(), "Contact email is not visible."
finally:
driver.quit()

2.

test_count_judges.py
from selenium import webdriver
from selenium.webdriver.common.by import By
from webdriver_manager.chrome import ChromeDriverManager
import time
def test_count_judges():
driver = webdriver.Chrome(ChromeDriverManager().install())
try:
driver.get("https://www.devchallenge.it")
# Navigate to Judges page
judges_menu = driver.find_element(By.LINK_TEXT, "DEV Challenge")
judges_menu.click()
time.sleep(2)
judges_page_link = driver.find_element(By.LINK_TEXT, "Judges")
judges_page_link.click()
time.sleep(2)
# Count judges
judges = driver.find_elements(By.CSS_SELECTOR, ".judge-card")  # Adjust the selector based on actual HTML
assert len(judges) == 6, "Expected number of judges is not 6."
finally:
driver.quit()

3.

test_no_mobile_partners.py
from selenium import webdriver
from selenium.webdriver.common.by import By
from selenium.webdriver.chrome.service import Service
from webdriver_manager.chrome import ChromeDriverManager
from selenium.webdriver.chrome.options import Options
import time
def test_no_mobile_partners():
chrome_options = Options()
chrome_options.add_argument("--window-size=360,800")  # Set mobile viewport
driver = webdriver.Chrome(service=Service(ChromeDriverManager().install()), options=chrome_options)
try:
driver.get("https://www.devchallenge.it")
# Navigate to Partners page
partners_menu = driver.find_element(By.LINK_TEXT, "DEV Challenge")
partners_menu.click()
time.sleep(2)
partners_page_link = driver.find_element(By.LINK_TEXT, "Partners")
partners_page_link.click()
time.sleep(2)
# Verify Apple Inc is NOT in the partners list
partners = driver.find_elements(By.CSS_SELECTOR, ".partner-name")  # Adjust the selector based on actual HTML
partner_names = [partner.text for partner in partners]
assert "Apple Inc" not in partner_names, "Apple Inc is listed among partners."
finally:
driver.quit()
