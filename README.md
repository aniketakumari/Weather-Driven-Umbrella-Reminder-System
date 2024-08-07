# **Weather-Driven Umbrella Reminder System**

## **Overview**

Welcome to the **Weather-Driven Umbrella Reminder System**! This Python-based project scrapes weather data to ensure you're always prepared for rainy or cloudy days. Using a combination of web scraping and email notifications, this system sends you an umbrella reminder if the forecast predicts less-than-ideal weather. The project employs Python libraries for web scraping, email handling, and scheduling tasks, making it a practical tool for daily weather updates.

## **Features**

- **Weather Scraping:** Gathers real-time weather data from Google.
- **Email Alerts:** Sends an automatic "umbrella reminder" email if the forecast includes rain or clouds.
- **Daily Scheduling:** Ensures you receive the reminder at the same time every day.

## **Tools & Libraries**

1. **Beautiful Soup (bs4):** A library for parsing HTML and XML documents. It provides Pythonic idioms for iterating, searching, and modifying the parse tree.
   - **Installation:** `pip install bs4`
   - **Usage:** `BeautifulSoup` is used to extract weather data from the HTML content retrieved from Google.

2. **Requests:** A simple HTTP library for Python, designed to make HTTP requests more accessible and more human-friendly.
   - **Installation:** `pip install requests`
   - **Usage:** `requests.get()` is used to send a GET request to Google, which retrieves the HTML content of the weather page.

3. **smtplib:** A Python module for sending emails using the Simple Mail Transfer Protocol (SMTP). It supports various email protocols and can be used to communicate with email servers.
   - **Installation:** `pip install smtplib` (typically included with Python's standard library)
   - **Usage:** `smtplib.SMTP()` is used to create an SMTP object, which facilitates the sending of email messages.

4. **Schedule:** A Python library for scheduling tasks. It allows you to specify when and how frequently tasks should run.
   - **Installation:** `pip install schedule`
   - **Usage:** `schedule.every().day.at("06:00").do(umbrellaReminder)` schedules the `umbrellaReminder()` function to run daily at 6:00 AM.

## **Code Explanation**

### **Step 1: Import Libraries**
```python
import schedule 
import smtplib 
import requests 
from bs4 import BeautifulSoup
```
- **schedule:** For scheduling tasks to run at specific times.
- **smtplib:** For sending email notifications.
- **requests:** For fetching HTML content from the web.
- **BeautifulSoup:** For parsing HTML content and extracting weather data.

### **Step 2: Fetch Weather Data**
```python
city = "Hyderabad"
url = "https://www.google.com/search?q=" + "weather" + city 
html = requests.get(url).content 
soup = BeautifulSoup(html, 'html.parser') 
temperature = soup.find('div', attrs={'class': 'BNeawe iBp4i AP7Wnd'}).text 
time_sky = soup.find('div', attrs={'class': 'BNeawe tAd8D AP7Wnd'}).text 
sky = time_sky.split('\n')[1]
```
- **city:** Sets the city for which weather data will be fetched.
- **url:** Constructs the Google search URL for the weather of the specified city.
- **requests.get(url).content:** Retrieves the HTML content of the weather page.
- **BeautifulSoup:** Parses the HTML and extracts temperature and sky conditions.
- **soup.find(...).text:** Finds the relevant HTML elements and extracts their text.

### **Step 3: Send Email Reminder**
```python
if sky in ["Rainy", "Rain And Snow", "Showers", "Haze", "Cloudy"]: 
    smtp_object = smtplib.SMTP('smtp.gmail.com', 587) 
    smtp_object.starttls() 
    smtp_object.login("YOUR EMAIL", "PASSWORD") 
    subject = "Umbrella Reminder"
    body = f"Take an umbrella before leaving the house.\nWeather condition for today is {sky} and temperature is {temperature} in {city}." 
    msg = f"Subject:{subject}\n\n{body}\n\nRegards,\nWeather Reminder System".encode('utf-8') 
    smtp_object.sendmail("FROM EMAIL", "TO EMAIL", msg) 
    smtp_object.quit() 
    print("Email Sent!")
```
- **if sky in [...]**: Checks if the weather conditions require an umbrella.
- **smtplib.SMTP('smtp.gmail.com', 587):** Creates an SMTP object for Gmail.
- **smtp_object.starttls():** Upgrades the connection to a secure encrypted SSL/TLS connection.
- **smtp_object.login(...):** Logs in to the Gmail account.
- **msg:** Constructs the email message with a subject and body.
- **smtp_object.sendmail(...):** Sends the email.
- **smtp_object.quit():** Closes the SMTP session.

### **Step 4: Schedule Daily Execution**
```python
schedule.every().day.at("06:00").do(umbrellaReminder) 

while True: 
    schedule.run_pending()
```
- **schedule.every().day.at("06:00").do(umbrellaReminder):** Schedules the `umbrellaReminder()` function to run every day at 6:00 AM.
- **while True:** Keeps the script running continuously to check for scheduled tasks.
- **schedule.run_pending():** Executes any pending scheduled tasks.

## **Future Scope**

1. **Multi-City Support:** Enhance the script to handle weather reminders for multiple cities by integrating user input for different locations.
2. **Advanced Forecasting:** Incorporate more detailed weather forecasting data from APIs to provide additional recommendations (e.g., specific weather alerts or packing tips).
3. **User Interface:** Develop a graphical user interface (GUI) to make configuration and interaction more user-friendly, allowing users to customize reminder settings.
4. **Notification Options:** Extend functionality to include SMS or mobile push notifications in addition to email alerts.
5. **Integration with Smart Devices:** Explore integrations with smart home devices to provide real-time alerts and automation based on weather conditions.

