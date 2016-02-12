---
layout: post
title: Setting up Jupyter for Serving External Users
---

Recently I've got my hands on a brand new RPi-2 from CanaKit. After checking it
operates properly, the first thing I wanted to was to setup Jupyter REPL for
real-time experiments.

Though, this turned out to be  no easy task.

First thing I did was creating a `virtualenv` for Jupyter. I prefer python packages to be isolated per project, instead of installing global packages every time. This was done in my home folder.

```bash
virtualenv jupyter -p python3
source ./jupyter/bin/activate
```
Once the environment is activated, `jupyter` was then installed.
```bash
pip3 install jupyter
```

I went ahead and start the notebook instance. Everything appeared fine, though it failed to find a browser that can open the notebook. This was expected since I was *inside a SSH session*.

However, when I tried to access the notebook from my VM, I got a `404`. =(

I first thought that perhaps port **8888** (Jupyter Notebook's default port) was not open to public, so I enabled it with the following command. (Though, realistically speaking I should've just checked this with `netstat -a`)

```bash
sudo iptables -A INPUT -p tcp --dport (8888) -j ACCEPT
netstat -lepunt
--
Proto Recv-Q Send-Q Local Address           Foreign Address         State       User       Inode       PID/Program name
tcp        0      0 0.0.0.0:22              0.0.0.0:*               LISTEN      0          6845        -               
tcp        0      0 0.0.0.0:8888            0.0.0.0:*               LISTEN      1001       720743      13906/python3.4
tcp6       0      0 :::22                   :::*                    LISTEN      0          6847        -               
tcp6       0      0 :::8888                 :::*                    LISTEN      1001       720744      13906/python3.4
```

So it appears that 8888 is indeed listening. What went wrong?

Turns out, Jupyter itself restricts access to **localhost** by default, and external
access needs to be enabled with configuration.

This config file is located under `~/.jupyter/jupyter_notebook_config.py`. The settings that need to be changed are

* `c.NotebookApp.certfile` = (an openssl cert file location)
* `c.NotebookApp.ip` = '\*' (to allow external access)
* `c.NotebookApp.keyfile` = (an openssl keyfile location)
* `c.NotebookApp.password` = (salted & hashed password)

> While the other 3 settings beside `ip` are completely optional, do note that your notebook will be vulnerable to attacks from the public with the security measures.
> You can acquire a SSL certificate through trusted vendors, or create one yourself with
> ```bash
>  openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout
~/.ssl/jupyter.key -out ~/.ssl/jupyter.pem
> ```

> You can also create a salted & hashed password by opening the iPython interpreter, import do the following:
> ```python3
> from notebook.auth import passwd
> passwd()
> ```
> which will proceed to prompt for your customized password. **You will use this password to login to Notebook, so make sure you remember it as only the salted&hashed version will persist in memory.**
> After you've provided your password, a hashed string will be created. Plug that into `c.NotebookApp.password`.

With all that modification, save the `jupyter_notebook_config.py` and restart your jupyter notebook server. You should now be able to login externally and access the Python3 kernal on your serving machine (in my case, I get RPi's GPIO access!)
