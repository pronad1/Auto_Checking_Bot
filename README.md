# 🔹 Step 1: Install Python and Required Libraries
1. Install Python (if not installed): https://www.python.org/downloads/
2. Open Command Prompt and run:
```
pip install selenium pandas openpyxl
```
# 🔹 Step 2: Prepare Your Excel File
Create a file named credentials.xlsx with the following structure:

| Email               | Password  |
|---------------------|-----------|
| user1@example.com   | pass123   |
| user2@example.com   | secret456 |



Save it in the same folder as your Python script.

# 🔹 Step 3: Download ChromeDriver
1. Go to: https://sites.google.com/chromium.org/driver/
2. Download the version that matches your Google Chrome browser version.
3. Extract the file and rename it to chromedriver.exe.
4. Place it in the same folder as your script or add its path in the code.

# 🔹 Step 4: Use This Python Script
```
import pandas as pd
from selenium import webdriver
from selenium.webdriver.common.by import By
import time

# Load email-password list
df = pd.read_excel("credentials.xlsx")

# Store successful logins
successful_logins = []

# Set path and login URL
driver_path = "chromedriver.exe"
login_url = "https://example.com/login"  # ✅ Replace with your actual site

# Loop through each email-password pair
for index, row in df.iterrows():
    email = row['Email']
    password = row['Password']

    # Launch browser
    driver = webdriver.Chrome(executable_path=driver_path)
    driver.get(login_url)
    time.sleep(2)

    try:
        # Adjust selectors as per your site's HTML
        email_input = driver.find_element(By.NAME, "email")
        password_input = driver.find_element(By.NAME, "password")
        login_button = driver.find_element(By.XPATH, "//button[@type='submit']")

        # Perform login
        email_input.send_keys(email)
        password_input.send_keys(password)
        login_button.click()

        time.sleep(3)  # Wait for login to complete

        # ✅ Define success condition (customize as per your website)
        if "dashboard" in driver.current_url or "logout" in driver.page_source:
            print(f"✅ Success: {email}")
            successful_logins.append(email)
        else:
            print(f"❌ Failed: {email}")

    except Exception as e:
        print(f"⚠️ Error with {email}: {e}")
    finally:
        driver.quit()

# Save successful emails to Excel
if successful_logins:
    pd.DataFrame(successful_logins, columns=["Email"]).to_excel("successful_logins.xlsx", index=False)
    print("\n✅ Saved successful logins to 'successful_logins.xlsx'")
else:
    print("\n❌ No successful logins found.")
```
# 🔹 Step 5: Run the Script
1. Save the script as auto_login_bot.py
2. Open a terminal in the same directory.
3. Run:
 ```
python auto_login_bot.py
```
# 🔹 Step 6: Get the Output
1. After the script runs, you’ll find:
2. successful_logins.xlsx file in the same folder.
3. It will contain only the emails that logged in successfully.
