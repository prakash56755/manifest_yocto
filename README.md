Install Yocto development Environment

Use ubuntu 22.04 or ubuntu 20.04

Install the necessary packages in ubuntu:

sudo apt-get update

sudo apt-get install -y apt-utils openssh-server mc libgmp3-dev libmpc-dev gawk wget git diffstat unzip texinfo gcc \
build-essential chrpath socat cpio python3 python3-pip python3-pexpect xz-utils debianutils iputils-ping \
python3-git python3-jinja2 libegl1-mesa libsdl1.2-dev pylint3 xterm python3-subunit mesa-common-dev zstd \
liblz4-tool git fakeroot build-essential ncurses-dev xz-utils libssl-dev bc flex libelf-dev bison xinetd \
tftpd tftp nfs-kernel-server libncurses5 libc6-i386 libstdc++6-i386-cross libgcc1-i386-cross lib32z1 \
device-tree-compiler curl mtd-utils u-boot-tools net-tools swig rust-all  tmux

Install repo:

sudo curl http://commondatastorage.googleapis.com/git-repo-downloads/repo > /usr/bin/repo
sudo chmod a+x /usr/bin/repo

if you want to use Dockerfile to compile yocto.

git clone https://github.com/prakash56755/docker_yocto.git

cd docker_yocto

Docker build:

docker build -t qemu_yocto --build-arg username=yocto .

Docker Run:

docker run -it --rm --name=yocto_container --ipc=host --user=$UID --volume $(pwd):/home/yocto -w /home/yocto qemu_yocto

Fetch yocto source:

mkdir -p /home/user/var-qemuarm64

cd /home/user/var-qemuarm64

repo init -u https://github.com/prakash56755/manifest_yocto -b kirkstone -m kirkstone-var-qemu-1-1.0.xml
 
repo sync -j24

cd source

source oe-init-build-env build-varisicte-qemuarm64

After the above step it will create a directory build-varisicte-qemuarm64 

Then open conf/local.conf , then change the Machine name to compile the variscite-qemu64

change the machine name from MACHINE ??= "qemux86-64" to 

MACHINE ??= "variscite-qemuarm64"

Then add the custom layer in bblayer.conf using the below command

 bitbake-layers add-layer ../meta-variscite-qemu/
 
 bitbake-layers add-layer ../meta-openembedded/meta-oe/
 
 bitbake-layers add-layer ../meta-openembedded/meta-python/
 
 bitbake-layers add-layer ../meta-openembedded/meta-networking/

Now start the building the image using the below command.

bitbake var-qemu-console-image


