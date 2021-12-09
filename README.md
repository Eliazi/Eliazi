import smtplib
import os
from email.mime.multipart import MIMEMultipart
from email.mime.text import MIMEText
from pathlib import Path

report_file = open(Path("Backup.html"))
# alert_msg = MIMEText(report_file.read(),"html", "utf-8")

# me == my email address
# you == recipient's email address

you = "eliazi@altair-semi.com"
me = "it@altair-semi.com"
subject = 'test mail from eliaz'

# Create message container - the correct MIME type is multipart/alternative.
msg = MIMEMultipart('alternative')
msg['Subject'] = subject
msg['From'] = me
msg['To'] = you

# Create the body of the message (a plain-text and an HTML version).
text = "Hi!\nHow are you?\nHere is the link you wanted:\nhttps://www.python.org"
html = report_file.read()

# Record the MIME types of both parts - text/plain and text/html.
part1 = MIMEText(text, 'plain')
part2 = MIMEText(html, 'html')

# Attach parts into message container.
# According to RFC 2046, the last part of a multipart message, in this case
# the HTML message, is best and preferred.
msg.attach(part1)
msg.attach(part2)

server = smtplib.SMTP('outbound-eu1.ppe-hosted.com')

server.send_message(msg)
# server.sendmail(me,you,msg.as_string())
server.quit()
