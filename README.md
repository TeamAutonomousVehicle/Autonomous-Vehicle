### HARDWARE Requirements
1. Raspberry Pi 3b+ : https://www.raspberrypi.com/products/raspberry-pi-3-model-b-plus/
2. WebCam : https://www.amazon.in/Logitech-C270-HD-Webcam-Black/dp/B008QS9J6Y
3. SG 90 servo: https://www.amazon.in/Robodo-Electronics-Tower-Micro-Servo/dp/B00MTFFAE0
4. PCA9685 Servo Driver: https://www.amazon.in/Robodo-Electronics-Tower-Micro-Servo/dp/B00MTFFAE0

### SOFTWARES
1. Raspian Legacy (Buster) OS : https://downloads.raspberrypi.org/raspios_oldstable_lite_armhf/images/raspios_oldstable_lite_armhf-2021-12-02/2021-12-02-raspios-buster-armhf-lite.zip
2. Etcher Image Burner: https://www.balena.io/etcher/
3. MOBA Xterm: https://mobaxterm.mobatek.net/download.html

### PROCEDURE
1. Burn OS
2. Create empty file in boot partition ```ssh```
3. Create ```wpa_supplicant.conf``` file in boot partition
    which contains this:
    ```
    country=US
    ctrl_interface=DIR=/var/run/wpa_supplicant GROUP=netdev
    update_config=1

    network={
        ssid="WIFI_USERNAME"
        psk="WIFI_PASSWORD"
    }

    ```
4. Eject the SD card and inseart to Raspberry Pi
5. Switch ON the Raspberry pi.
6. open Terminal in PC and type ```ping raspberrypi.local``` and note down IP address of Raspberry Pi
7. open MOBA Xterm, Click Session -> SSH and type IP Address in host field and click ok.
8. Login using default credientials 
    ```
    Username: pi
    Password: raspberry
    ```
9. 
