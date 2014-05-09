---
layout: post
title: "python, mail"
description: "python发送邮件"
category: python
tags: [python, mail, smtplib, SMTP, SMTP_SSL]
---
{% include JB/setup %}

利用python发送邮件（smtp服务器有账号权限控制）

<pre class="code prettyprint linenums">
#!/bin/env python
#-*- coding: utf-8 -*-
import os
import hashlib
import smtplib
from email.Header import Header
from email.MIMEMultipart import MIMEMultipart
from email.MIMEText import MIMEText
from email.MIMEImage import MIMEImage
from email.Utils import formatdate

SMTP_SERVER = 'smtp.xx.com'
SMTP_PORT = 465
USERNAME = 'x@xx.com'
PASSWORD = '123456'

# 收件人含中文情况下，需要处理，避免出现乱码
def encode(mail_to):
    name = mail_to[:mail_to.find('&lt;')]
    mail = mail_to[mail_to.find('&lt;'):]
    mail_to = str(Header(name + mail, 'utf-8'))
    return mail_to

def make_mail_to(mail_to):
    mail_list = mail_to.split(',')
    mail_list = map(lambda x: encode(x), mail_list)
    return ','.join(mail_list)

def send(subject, content, mail_to, img_list):
    '''
        @param mail_to: 发送对象，name1&lt;x@xx.com>,name2&lt;x@xx.com>
        @param img_list: 包含的图片的路径列表
    '''
    
    msg = MIMEMultipart()
    msg['From'] = USERNAME
    msg['To'] = make_mail_to(mail_to)
    msg['Date'] = formatdate(localtime = True)
    msg['Subject'] = Header(subject, 'utf-8')

    if len(content) > 0 : 
        msg_html = MIMEText(content, 'html', 'utf-8')
        msg.attach(msg_html)
        if img_list and isinstance(img_list, list):
            for img in img_list:
                if not img or not os.path.exists(img):
                    continue
                fp = open(img, 'rb')
                msg_img = MIMEImage(fp.read())
                fp.close()
                # 引用的时候以文件路径的md5值为id
                msg_img.add_header('Content-ID', hashlib.md5(img).hexdigest())
                msg.attach(msg_img)

    # 需要账号验证
    server = smtplib.SMTP_SSL(SMTP_SERVER, SMTP_PORT)
    #server.set_debuglevel(1)
    server.ehlo()
    server.login(USERNAME, PASSWORD)
    server.sendmail(USERNAME, mail_to.split(","), msg.as_string())
    server.quit

if __name__ =='__main__':
    send('subject', 'content', 'hellojaf@gmail.com')
</pre>

如果不需要账号验证，可以直接使用smtp.sendmail方法：

<pre class="code prettyprint linenums">
    smtp = smtplib.SMTP(SMTP_SERVER)
    smtp.sendmail(USERNAME, mail_to.split(","), msg.as_string())
    smtp.close()
</pre>