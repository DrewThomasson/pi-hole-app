# pi-hole-app
I love pi-hole but I wanted an easy way to launch/install locally so I made this

<img width="163" alt="image" src="https://github.com/user-attachments/assets/321f6e9d-781a-4049-a746-f2b791099499" />


### Requirments: 
Running Mac OS and Docker Desktop installed

Make sure this is the dns for the wifi your connecting to for this to work. (In System Settings)
`127.0.0.1`

### How to make app

Just paste this command in your terminal to create the app on your Desktop (you can then move it into the applications folder)

```bash
# Create the application bundle structure
mkdir -p ~/Desktop/PiHole.app/Contents/MacOS
mkdir -p ~/Desktop/PiHole.app/Contents/Resources

# Create the Info.plist file
cat > ~/Desktop/PiHole.app/Contents/Info.plist << 'EOF'
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
    <key>CFBundleExecutable</key>
    <string>PiHole</string>
    <key>CFBundleIdentifier</key>
    <string>com.user.pihole</string>
    <key>CFBundleName</key>
    <string>PiHole</string>
    <key>CFBundleVersion</key>
    <string>1.0</string>
    <key>CFBundlePackageType</key>
    <string>APPL</string>
</dict>
</plist>
EOF

# Create the executable script
cat > ~/Desktop/PiHole.app/Contents/MacOS/PiHole << 'EOF'
#!/bin/bash
osascript -e 'tell application "Terminal" to do script "docker run -d --name pihole -e ServerIP=192.168.0.10 -e TZ=Europe/Berlin -e WEBPASSWORD=password -e DNS1=1.1.1.1 -e DNS2=1.0.0.1 -p 80:80 -p 53:53/tcp -p 53:53/udp -p 443:443 -v ~/pihole/:/etc/pihole/ --dns=127.0.0.1 --dns=1.1.1.1 --cap-add=NET_ADMIN --restart=unless-stopped pihole/pihole:latest"'
EOF

# Make the script executable
chmod +x ~/Desktop/PiHole.app/Contents/MacOS/PiHole

echo "PiHole.app created on your desktop! Double-click it to run your Docker command."
```

### Extra info

pi-hole data will be located at 
`~/pihole`

The password will randomly created at launch and be within the logs
To view the docker logs 
-> to see the generated pasword after first launch
```bash
docker logs pihole
```

Make sure this is the dns for the wifi your connecting to for this to work. (In System Settings)
`127.0.0.1`


Launch command this app runs:

```bash
docker run -d --name pihole -e ServerIP=192.168.0.10 -e TZ=Europe/Berlin -e WEBPASSWORD=password -e DNS1=1.1.1.1 -e DNS2=1.0.0.1 -p 80:80 -p 53:53/tcp -p 53:53/udp -p 443:443 -v ~/pihole/:/etc/pihole/ --dns=127.0.0.1 --dns=1.1.1.1 --cap-add=NET_ADMIN --restart=unless-stopped pihole/pihole:latest
```

