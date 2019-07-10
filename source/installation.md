# Installation for Developers

## Copy repositories
Splash contains two repositories: server and client.

* Make a directory for splash
```
mkdir splash
```

* Clone repositories

```
git clone https://github.com/als-computing/splash-client.git
python3 -m venv env
source env/bin/activate
git clone https://github.com/als-computing/splash-server.git
cd splash-server
pip install -e .
```
The splash setup.py creates an entrypoint called "splash". So, to start the splash server in developement mode, type:
```
splash
```

#Installation for Production
[More to come...]