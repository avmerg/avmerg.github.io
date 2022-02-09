---
layout: post
title: Setting up Anaconda, ChromeDriver, TensorFlow and Selenium on EC2
subtitle: 
cover-img: /assets/img/path.jpg
thumbnail-img: /assets/img/thumb.png
share-img: /assets/img/path.jpg
tags: [python, data science]
---

In this blog post, I'm going to walk you through getting Selenium to run on AWS to run a bot. 

This information was surprisingly hard to find online. You may want to look at SageMaker for a potentially easier solution to this problem but this is how I did it!

** Preparing server

* Let's get conda on there!

    wget https://repo.continuum.io/archive/Anaconda3-2018.12-Linux-x86_64.sh

* update conda, install pip
    conda update conda
    conda install pip

* Install requirements for the bot
    conda install --yes --file requirements.txt

* Get updated packages and their dependencies
    sudo apt update
    sudo apt install libgl1-mesa-glx


    sudo curl -sS -o - https://dl-ssl.google.com/linux/linux_signing_key.pub | sudo  apt-key add
    echo 'deb [arch=amd64]  http://dl.google.com/linux/chrome/deb/ stable main" | sudo tee -a /etc/apt/sources.list.d/google-chrome.list

    sudo echo "deb [arch=amd64]  http://dl.google.com/linux/chrome/deb/ stable main" >> /etc/apt/sources.list.d/google-chrome.list

    sudo echo "deb [arch=amd64]  http://dl.google.com/linux/chrome/deb/ stable main" >> /etc/apt/sources.list.d/google-chrome.list

    echo 'deb http://www.duinsoft.nl/pkg debs all' | sudo tee -a /etc/apt/sources.list

    echo "deb [arch=amd64]  http://dl.google.com/linux/chrome/deb/ stable main" | sudo tee -a /etc/apt/sources.list.d/google-chrome.list


* Get selenium running!

    xvfb-run java -Dwebdriver.chrome.driver=/usr/bin/chromedriver -jar /mnt/server_bot/Bot/selenium-server-standalone-3.13.0.jar

* Schedule your cron jobs and run the bot on the TensorFlow conda environment

    #*/3 * * * * conda activate tf; export DISPLAY=:0 && export PATH=$PATH:/usr/local/bin && python /mnt/server_bot/Bot/

    */2 * * * * /home/ubuntu/anaconda3/bin/python /mnt/server_bot/Bot/test.py