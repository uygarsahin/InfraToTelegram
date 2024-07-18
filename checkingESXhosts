#!/usr/bin/env python
# coding: utf-8

'''
Author: Uygar Sahin
Vers  : 1.1
Info  : This code scan 4 Vcenter then Code check ESX hosts's cpu and Memory utilizations.
If cpu and memeory usage are over 85 percent send message via Telegram and log to log file.
The code use telegram api for sending massage.
-----------------------------------------------------
Release Note:
V1.1:
- ESX hosts status check line add to code.
- Telegram sending message problem resolve.
    * Send messeage every 3 second. Because Telegram request is not allow more message in second.

'''
import ssl
import base64
import logging
import logging.config
from pyVim.connect import SmartConnect
from pyVmomi import vim
import telebot
import datetime
import time


ssl_context = ssl.create_default_context(purpose=ssl.Purpose.CLIENT_AUTH)
ssl_context.verify_mode = ssl.CERT_NONE

# logging parameters
logFileName="/tmp/Check_ESXhosts.log"
logging.basicConfig(filename=logFileName,format='%(asctime)s %(message)s',filemode='a')
logger = logging.getLogger()

# Telegram API token ve chat ID Information
telegram_token = "TELEGRAM_TOKEN"
telegram_chat_id = "TELEGRAM_CHAT_ID"

vcs=["ESX1_FQDN","ESX2_FQDN","ESX3_FQDN","ESX4_FQDN"]
# Critical CPU/Memory threshold value
cpu_threshold = 90     # percent of threshold value
memory_threshold = 85  # percent of threshold value

try:
    for vcip in vcs:
        si = SmartConnect(host=vcip, user=base64.b64decode('VCENETER_USER_with_BS64_Encode').decode()+"@company.cm", pwd=base64.b64decode('VcenterUser_PASSWORD_With_BS64_Encode').decode(),sslContext=ssl_context)
        content = si.RetrieveContent()
        host_view = content.viewManager.CreateContainerView(content.rootFolder, [vim.HostSystem], True)
        #print(host_view)
        hosts = host_view.view
        host_view.Destroy()

        for host in hosts:
        # ESXi Host Name
            host_name = host.summary.config.name
            host_MaintenanceState= host.runtime.inMaintenanceMode
            host_OverallStatus=host.summary.overallStatus

            # CPU utilization of ESXi Host
            cpu_usage = float(host.summary.quickStats.overallCpuUsage)
            cpu_total = float(((host.summary.hardware.cpuMhz * host.summary.hardware.numCpuCores)))
            cpu_usage_percent = int(round(((cpu_usage/cpu_total)*100),0))
            #print(host_name,":",cpu_usage_percent,":",host_OverallStatus)

            # Memory utilization of ESXi Host
            mem_usage = ((float(host.summary.quickStats.overallMemoryUsage)/1024))
            mem_total = ((float(host.summary.hardware.memorySize)/1024/1024/1024))
            memory_usage_percent = int(round(((mem_usage/mem_total)*100),0))


            # Check the threshold value then Send message to Telegram
            if cpu_usage_percent >= cpu_threshold:
                message = f"{host_name} host CPU usage is at {cpu_usage_percent} %. "+datetime.datetime.now().strftime("%Y-%m-%d %H:%M")
                logger.critical(message)
                bot = telebot.TeleBot(telegram_token)
                bot.send_message(telegram_chat_id, message)
                time.sleep(2) # wait after send message second.

            if memory_usage_percent >= memory_threshold:
                message = f"{host_name} host memory usage is at {memory_usage_percent} %. "+datetime.datetime.now().strftime("%Y-%m-%d %H:%M")
                logger.critical(message)
                bot = telebot.TeleBot(telegram_token)
                bot.send_message(telegram_chat_id, message)
                time.sleep(2) # wait after send message second.

            # Check the status of ESxi Host. If it is not green, the code send a message to Telegram.
            #if host_OverallStatus != "green":
            if host_OverallStatus == "red":
                message = f"{host_name} host status is {host_OverallStatus}. "+datetime.datetime.now().strftime("%Y-%m-%d %H:%M")
                logger.critical(message)
                bot = telebot.TeleBot(telegram_token)
                bot.send_message(telegram_chat_id, message)
                time.sleep(2) # wait after send message second.
except:
    logger.critical("Some Error When Code running")
    bot = telebot.TeleBot(telegram_token)
    bot.send_message(telegram_chat_id, "Some Error When Code running")
