Automated Bot to send Emails And Set reminders

I have created an automated bot that reminds the user to their certain task such as eating food , exercising etc through the compiler or by sending the Email.

Given Below is the code for it,

!pip install schedule

import schedule
import time
import ipywidgets as widgets
from IPython.display import display
from datetime import datetime

import smtplib
from email.mime.text import MIMEText
from email.mime.multipart import MIMEMultipart



SMTP_SERVER = 'smtp.gmail.com'
SMTP_PORT = 587
EMAIL_ADDRESS = 'akhivalo77@gmail.com'
EMAIL_PASSWORD = 'AkhileshPatil1212'

def set_Reminder(task_name):
    print(f"Reminder :  {task_name} Time : {datetime.now()}")


def daily_summary(task_name):
  print(f"Daily Summary : {task_name} Time :  {datetime.now()} ")


def send_email_reminder(task_name):
    print(f"Sending email for: {task_name} at {datetime.now()}")
    msg = MIMEMultipart()
    msg['From'] = EMAIL_ADDRESS
    msg['To'] = EMAIL_ADDRESS
    msg['Subject'] = f"Task Reminder: {task_name}"
    body = f"Hi, this is a reminder for your task: {task_name} at {datetime.now()}."
    msg.attach(MIMEText(body, 'plain'))

    try:
        with smtplib.SMTP(SMTP_SERVER, SMTP_PORT) as server:
            server.starttls()
            server.login(EMAIL_ADDRESS, EMAIL_PASSWORD)
            server.sendmail(EMAIL_ADDRESS, EMAIL_ADDRESS, msg.as_string())
        print("Email sent successfully.")
    except Exception as e:
        print(f"Failed to send email: {e}")

#Creating the user inferface now

task_name_input  =  widgets.Text(placeholder = "Enter Task  : " , description = "Task : ")
task_type_dropdown = widgets.Dropdown(
    options= ["Reminder" , "Daily Summary"  , "Email Reminder"],
    value = "Reminder",
    description  = "Type : "
)

time_interval= widgets.IntText(value=10, description="Interval (mins):")


button = widgets.Button(description ="Schedule Tasks")
display(task_name_input, task_type_dropdown, time_interval, button)


def schedule_task(button):
    task_name = task_name_input.value
    task_type = task_type_dropdown.value
    interval = time_interval.value

    if task_type == "Reminder":
        schedule.every(interval).minutes.do(set_Reminder, task_name)
    elif task_type == "Daily Summary":
        schedule.every(interval).minutes.do(daily_summary, task_name)
    elif task_type == "Email Reminder":
        schedule.every(interval).minutes.do(send_email_reminder, task_name)


    print(f"Task '{task_name}' scheduled as '{task_type}' every {interval} minutes.")


button.on_click(schedule_task)
print("Scheduler started...")
