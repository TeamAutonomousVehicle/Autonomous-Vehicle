### HARDWARE Requirements
1. Raspberry Pi 3b+ : https://www.raspberrypi.com/products/raspberry-pi-3-model-b-plus/
2. WebCam : https://www.amazon.in/Logitech-C270-HD-Webcam-Black/dp/B008QS9J6Y
3. SG 90 servo: https://www.amazon.in/Robodo-Electronics-Tower-Micro-Servo/dp/B00MTFFAE0
4. PCA9685 Servo Driver: https://www.amazon.in/Robodo-Electronics-Tower-Micro-Servo/dp/B00MTFFAE0

### HARDWARE CONNECTIONS
1. WEBCAM/PICAM to raspberrypi
![image](https://user-images.githubusercontent.com/44223447/161432396-7937b7c2-b791-49eb-937e-7802ad41a61e.png)
3. PCA9685
    GND - 6
    OE - NO CONN
    SCL - 5
    SDA - 3
    VCC - 4
    V+ - 2
    CH 0 - SERVO STEERING
    CH 1 - SERVO THROTTLE/SPEED


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
![image](https://user-images.githubusercontent.com/44223447/161415770-649f5abc-bbbb-48a8-b24c-1fe62f31c9d2.png)
![image](https://user-images.githubusercontent.com/44223447/161415793-58e1c816-725f-4afc-abf2-6e27f9cc50a3.png)

9. Execute these one by one
    ```
    sudo apt-get update --allow-releaseinfo-change
    sudo apt-get upgrade
    sudo raspi-config
    ```
    
    enable Interfacing Options - I2C
    enable Interfacing Options - Camera
    select Advanced Options - Expand Filesystem so you can use your whole sd-card storage
    Choose <Finish> and hit enter.

    Reboot after changing these settings.
    
10. Execute these one by one
    ```
    sudo apt-get install build-essential python3 python3-dev python3-pip python3-virtualenv python3-numpy python3-picamera python3-pandas python3-rpi.gpio i2c-tools avahi-utils joystick libopenjp2-7-dev libtiff5-dev gfortran libatlas-base-dev libopenblas-dev libhdf5-serial-dev libgeos-dev git ntp git curl libsdl2-mixer-2.0-0 libsdl2-image-2.0-0 libsdl2-2.0-0 libilmbase-dev libopenexr-dev libgstreamer1.0-dev libjasper-dev libwebp-dev libatlas-base-dev libavcodec-dev libavformat-dev libswscale-dev libqtgui4 libqt4-test python3-opencv -y
    
    (re execute above command if failed to get due to internet/http err)
    
    python3 -m virtualenv -p python3 env --system-site-packages
    echo "source env/bin/activate" >> ~/.bashrc
    source ~/.bashrc
    
    mkdir projects
    cd projects
    
    wget https://github.com/autorope/donkeycar/archive/refs/tags/4.2.1.zip
    unzip 4.2.1.zip
    mv donkeycar-4.2.1 donkeycar
    cd donkeycar
    pip install -e .[pi]
    
    pip install https://github.com/lhelontra/tensorflow-on-arm/releases/download/v2.2.0/tensorflow-2.2.0-cp37-none-linux_armv7l.whl
    
    cd ~
    donkey createcar --path ~/mycar
    cd ~/mycar
    
    sudo apt-get install -y i2c-tools
    sudo i2cdetect -y 1
    ```
![image](https://user-images.githubusercontent.com/44223447/161426378-721dc0a0-ca2e-4b0b-b8e7-2afa80722deb.png)

    ```
    donkey calibrate --channel 0 --bus=1    #Steering
    donkey calibrate --channel 1 --bus=1    #Throttle
    ```
![image](https://user-images.githubusercontent.com/44223447/161426432-8d4de459-f748-4d1e-ace5-4062c487a78f.png)

    ```
    Uncomment this and set to WebCam
    ```
![image](https://user-images.githubusercontent.com/44223447/161426580-ed4786e2-444d-426f-b6d3-cc2269dfdde7.png)
    ```
    Uncomment steering and throttle pwm and set as per calibration
    ```
![image](https://user-images.githubusercontent.com/44223447/161426602-d8f4fbf6-0e14-46b3-a229-44f5dab6ef65.png)
    ```
    Ctl+X then Y and enter.
    
    pip install pygame
     
    python manage.py drive
    ```
![image](https://user-images.githubusercontent.com/44223447/161426697-faa21afe-bb73-4cae-af56-18cc5e8e30d6.png)
    ```
    Go to raspberrypi.local:8887 and record the data. Use web controller/joystick
    ```
![image](https://user-images.githubusercontent.com/44223447/161426761-c7e83fd8-bdb9-429c-b844-32abaef39358.png)
    ```
    Recorded data will be saved in /home/pi/mycar/data/
    ```
![image](https://user-images.githubusercontent.com/44223447/161426843-0884dee0-5d7a-470a-9b9c-d60a46ffa1ad.png)

    
### Training the model
    ```
    cd ~/mycar
    mkdir tub_01_2022_04_03 #xx_yyyy_mm_dd
    cp -a ~/mycar/data/. ~/mycar/tub_01_2022_04_03/
    tar -czf tub_01_2022_04_03.tar.gz tub_01_2022_04_03
    ```
    
    Now copy paste the xx_yyyy_mm_dd.tar to your PC.
    Each time for new data, delete contents of mycar/data/images and other catalog,manifest,json files in mycar/data
    
    Now Follow:
    https://colab.research.google.com/drive/1Ci8lRAQrvIZ0vsg0Gr9s2yzp7CAWvcNy?usp=sharing
    
    
    Download Model both h5 and tflite and put in the mycar/model directory of raspberrypi
    then run the following:
    ```
    python manage.py drive --model ~/mycar/models/mypilot.h5
    python manage.py drive --model ~/mycar/models/mypilot4.tflite --type tflite_linear

    Go to raspberrypi.local:8887
    Select Mode:
    a. User : control of both the steering and throttle manual
    b. Local Angle : trained model  controls the steering
    c. Local Pilot : trained model control both the steering and the throttle
    ```
    
