# hardware-acceleration-ubuntu-20
This is a guide on how to turn on hardware acceleration in Ubuntu 20

For Reference: https://www.linuxuprising.com/2018/08/how-to-enable-hardware-accelerated.html

1. Add the Chromium with VA-API support PPA.

BETA BRANCE:
    $ sudo add-apt-repository ppa:saiarcot895/chromium-beta
OR
DEV BRANCE:
$ sudo add-apt-repository ppa:saiarcot895/chromium-dev

2. Pin the PPA with a priority of 1001.
for the Beta PPA:
$ sudo nano /etc/apt/preferences.d/chromium

add these lines:

for beta:
---
Package: *
Pin: release o=LP-PPA-saiarcot895-chromium-beta
Pin-Priority: 1002
---

for dev:
---
Package: *
Pin: release o=LP-PPA-saiarcot895-chromium-dev
Pin-Priority: 1002
---


3. Install Chromium Browser from the Saiarcot895 (VA-API) PPA:
$ sudo apt update
$ sudo apt install chromium-browser

4. Install the VA-API driver
For Intel graphics cards, you'll need to install the i965-va-driver package (it may already be installed):
$ sudo apt install i965-va-driver

For NVIDIA:
$ sudo apt install vdpau-va-driver
if missing. Download DEB from below: vdpau-va-driver_0.7.4-7ubuntu1~ppa2~20.04.1_amd64.deb	
http://ppa.launchpad.net/saiarcot895/chromium-dev/ubuntu/pool/main/v/vdpau-video/

5. Enable the Hardware-accelerated video option in Chromium.
Go to
$ chrome://flags
enable these:
ignore-gpu-blacklist
enable-gpu-rasterization
enable-zero-copy
enable-oop-rasterization

6. Install h264ify Chrome extension.
https://chrome.google.com/webstore/detail/h264ify/aleakchihdccplidncghkekgioiakgal
OR
https://chrome.google.com/webstore/detail/enhanced-h264ify/omkfmpieigblcllmkgbflkikinpkodlk

install these packages to see info:
$ sudo apt install vainfo
$ sudo apt install vdpauinfo

check output
$ vainfo
$ vdpauinfo

These should give output.

set this in environment variable:

For Intel:
$ sudo nano /etc/environment
LIBVA_DRIVER_NAME=i965
VDPAU_DRIVER=va_gl

For NVIDIA:
$ sudo nano /etc/environment
LIBVA_DRIVER_NAME=vdpau
VDPAU_DRIVER=nvidia

Restart or Logout pc.

Finally Run chromium
for intel:
$ chromium-browser --use-gl=desktop
OR
$ LIBVA_DRIVER_NAME=i965 chromium-browser --use-gl=desktop

for NVIDIA:
$ LIBVA_DRIVER_NAME=vdpau chromium-browser --use-gl=desktop

Check out if hardware acceleration working
Go to:
chrome://gpu
chrome://media-internals

Run this video in 1080p 60fps
https://www.youtube.com/watch?v=LXb3EKWsInQ

check chrome://media-internals
kVideoDecoderName	"MojoVideoDecoder" means hardware acceleration working. Anything else means not working.

Also check this test:
https://testdrive-archive.azurewebsites.net/performance/fishbowl/

I get 500+ fish in 60fps with my old GTX 850M 2 GB.

EXTRA:
Check if your browser supports DRM Protected content. Netflix stuff
https://demo.castlabs.com/
Click on the DRM video

If not working follow this 

=== Widevine Support ===

The packages in this PPA have support for Widevine inside Chromium enabled. However, you still need to copy some files from Chrome into Chromium for you to use Netflix (or other websites using EME) in Chromium.

1. Download and install Chrome (or extract the necessary files, if you know how to do that).
2. From the Chrome installation directory (probably /opt/google/chome or something similar), copy libwidevinecdm.so into ~/.config/chromium.
3. Restart Chromium.

FOR MORE: CHECK OUT THIS
https://launchpad.net/~saiarcot895/+archive/ubuntu/chromium-dev











