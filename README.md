from selenium import webdriver
from selenium.webdriver.common.by import By
from selenium.webdriver.common.keys import Keys
from selenium.webdriver.chrome.service import Service
from selenium.webdriver.chrome.options import Options
from selenium.webdriver.support.ui import WebDriverWait
from selenium.webdriver.support import expected_conditions as EC
import time
import os
from selenium.common.exceptions import WebDriverException, TimeoutException

# Replace these with your Twitter login credentials
username = "@HemanjaliP27517"  # Twitter username
email = "hemanjalipasangulapati@gmail.com"  # Your email for verification
password = "Anjali@123"  # Your password
tweet_text = "December, the last month of the year, brings a time to close chapters and start anew."

# Set up the WebDriver (Make sure the path to ChromeDriver is correct)
chromedriver_path = 'C:\\Users\\security1.madhapur\\Downloads\\chromedriver\\chromedriver-win64\\chromedriver.exe'

# Check if ChromeDriver path exists
if not os.path.exists(chromedriver_path):
    print("Error: ChromeDriver not found at the specified path.")
    exit(1)

# Set up Chrome options for a fresh browser instance
chrome_options = Options()
chrome_options.add_argument("--start-maximized")  # Start browser maximized
chrome_options.add_argument("--no-sandbox")
chrome_options.add_argument("--disable-dev-shm-usage")
chrome_options.add_argument("--disable-extensions")  # Disable unnecessary extensions
chrome_options.add_argument("--disable-gpu")  # Disable GPU hardware acceleration for stability

# Initialize WebDriver service
service = Service(chromedriver_path)
driver = webdriver.Chrome(service=service, options=chrome_options)

# Open Twitter login page
driver.get('https://twitter.com/login')

# Initialize WebDriverWait
wait = WebDriverWait(driver, 30)  # Increased wait time for robustness

# Step 1: Enter username (Twitter handle) and submit
try:
    username_field = wait.until(EC.presence_of_element_located((By.XPATH, '//input[@name="text" or @aria-label="Phone, email, or username"]')))
    username_field.send_keys(username)
    username_field.send_keys(Keys.RETURN)
    print("Entered username and submitted.")
except TimeoutException:
    print("Timeout: Username field not found.")
    driver.quit()
    exit(1)

# Step 2: Handle email verification (if asked)
try:
    # Check if email verification prompt exists
    email_field = wait.until(EC.presence_of_element_located((By.XPATH, '//input[@name="text" or @aria-label="Phone, email, or username"]')))
    email_field.send_keys(email)
    email_field.send_keys(Keys.RETURN)
    print("Entered email for verification and submitted.")
except TimeoutException:
    print("No email verification prompt detected. Continuing with password entry.")
    pass

# Step 3: Wait for the password field to load and enter the password
try:
    password_field = wait.until(EC.presence_of_element_located((By.XPATH, '//input[@name="password" or @aria-label="Password"]')))
    password_field.send_keys(password)
    password_field.send_keys(Keys.RETURN)
    print("Entered password and submitted.")
except TimeoutException:
    print("Timeout: Password field not found.")
    driver.quit()
    exit(1)

# Step 4: Handle CAPTCHA
try:
    # Look for a CAPTCHA prompt (this can vary depending on Twitter's CAPTCHA)
    captcha_prompt = wait.until(EC.presence_of_element_located((By.XPATH, "//div[contains(text(),'captcha')]")))
    print("CAPTCHA detected! Please solve it manually.")
    input("Press Enter after you solve the CAPTCHA manually...")
except TimeoutException:
    print("No CAPTCHA detected. Continuing with login.")

# Step 5: Wait for the profile icon (Account menu) to confirm successful login
try:
    profile_icon = wait.until(EC.presence_of_element_located((By.XPATH, '/html/body/div[1]/div/div/div[2]/header/div/div/div/div[2]/div/button/div/div[1]/div[2]/div/div[2]/div/div/div[4]/div')))
    print("Login successful!")
except TimeoutException:
    print("Login failed: Profile icon not found.")
    driver.quit()
    exit(1)

# Step 6: Navigate to the tweet box
try:
    tweet_button = wait.until(EC.presence_of_element_located((By.XPATH, '//a[@href="/compose/post"]')))
    tweet_button.click()
    print("Navigated to tweet composition area.")
except TimeoutException:
    print("Timeout: Tweet button not found.")
    driver.quit()

# Step 7: Compose and post the tweet
try:
    tweet_input = wait.until(EC.presence_of_element_located((By.XPATH, '/html/body/div[1]/div/div/div[1]/div[2]/div/div/div/div/div/div[2]/div[2]/div/div/div/div[3]/div[2]/div[1]/div/div/div/div[1]/div[2]/div/div/div/div/div/div/div/div/div/div/div/div/div[1]/div/div/div/div/div/div[2]/div/div/div/div')))
    tweet_input.send_keys(tweet_text)
    print(f"Composed tweet: {tweet_text}")

    tweet_submit_button = wait.until(EC.presence_of_element_located((By.XPATH, '/html/body/div[1]/div/div/div[1]/div[2]/div/div/div/div/div/div[2]/div[2]/div/div/div/div[3]/div[2]/div[1]/div/div/div/div[2]/div[2]/div/div/div/button[2]/div/span/span')))
    tweet_submit_button.click()
    print("Tweet posted successfully!")
except TimeoutException:
    print("Timeout: Error composing or submitting the tweet.")
    driver.quit()

# Step 8: Wait a few seconds before closing
time.sleep(5)  # Allow time for the tweet to be posted

# Close the browser
driver.quit()# Twitter-login-Automation
This is a Twitter Automated project using selenim and python.In this  login happens directly with credentials thar were mentioned in the source code and a tweet is also posted automatially.
