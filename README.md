# Jetson_Orin_NX_setup_manual


jepack을 설치해줍니다.
유선랜을 먼저 연결해줍니다.(저같은경우 다른 컴퓨터 인터넷을 공유하여 사용했습니다)

#현재  ip 기록해두기(추후 화면 먹통되거나 연결이 안될 때 ssh로 연결 필요)
ifconfig

sudo apt update
sudo apt install openssh-server network-manager timeshift curl vim wget

sudo systemctl enable NetworkManager
sudo systemctl start NetworkManager


# bravce 브라우저 설치

sudo curl -fsSLo /usr/share/keyrings/brave-browser-archive-keyring.gpg https://brave-browser-apt-release.s3.brave.com/brave-browser-archive-keyring.gpg

echo "deb [signed-by=/usr/share/keyrings/brave-browser-archive-keyring.gpg arch=arm64] https://brave-browser-apt-release.s3.brave.com/ stable main" | sudo tee /etc/apt/sources.list.d/brave-browser-release.list

sudo apt update
sudo apt install brave-browser

#ros humble 설치
locale  # check for UTF-8
sudo apt update && sudo apt install locales
sudo locale-gen en_US en_US.UTF-8
sudo update-locale LC_ALL=en_US.UTF-8 LANG=en_US.UTF-8
export LANG=en_US.UTF-8
locale  # verify settings
sudo apt install software-properties-common
sudo add-apt-repository universe
sudo apt update && sudo apt install curl -y
export ROS_APT_SOURCE_VERSION=$(curl -s https://api.github.com/repos/ros-infrastructure/ros-apt-source/releases/latest | grep -F "tag_name" | awk -F\" '{print $4}')
curl -L -o /tmp/ros2-apt-source.deb "https://github.com/ros-infrastructure/ros-apt-source/releases/download/${ROS_APT_SOURCE_VERSION}/ros2-apt-source_${ROS_APT_SOURCE_VERSION}.$(. /etc/os-release && echo ${UBUNTU_CODENAME:-${VERSION_CODENAME}})_all.deb"
sudo dpkg -i /tmp/ros2-apt-source.deb
sudo apt update
sudo apt install ros-humble-desktop
sudo apt install ros-dev-tools

vim ~/.bashrc
# source /opt/ros/humble/setup.bash 넣어주기

#혹시 몰라 현재 상태 백업
sudo timeshift --create

sudo apt-mark hold nvidia-l4t-*  # 이걸 해줘야 문제가 안생김. 필수!
sudo apt upgrade

# 제대로 작동하는지 재부팅해서 확인
sudo reboot


# Realtek 8188FU, 8188FTV 무선랜카드 드라이버 다운(이 방식이 제일 쉽고 잘 되는듯)
sudo add-apt-repository ppa:kelebek333/kablosuz
sudo apt-get update
sudo apt install rtl8188fu-dkms
sudo reboot

# 와이파이 잡히는지 확인


#학교 와이파이 설정할 떄 

WPA & WPA2 Enterprise
Tunneled TLS
비우기
비우기
[선택] No CA certificate is required
PPA
KNU 아이디
KNU 비밀번호


#vscode 설치
sudo apt update
sudo apt install software-properties-common apt-transport-https wget
wget -q https://packages.microsoft.com/keys/microsoft.asc -O- | sudo apt-key add -
sudo add-apt-repository "deb [arch=arm64] https://packages.microsoft.com/repos/vscode stable main"
sudo apt install code

#conda 설치
sudo apt-get install libgl1-mesa-glx libegl1-mesa libxrandr2 libxrandr2 libxss1 libxcursor1 libxcomposite1 libasound2 libxi6 libxtst6
curl -O https://repo.anaconda.com/archive/Anaconda3-2025.12-2-Linux-aarch64.sh

bash ~/Anaconda3-2025.12-2-Linux-aarch64.sh

