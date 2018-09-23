# rpi-i2s-audio

# Compile and Load I2S Module:

cd /root

/usr/bin/rpi-update

cp /boot/config.txt /boot/config.txt.bkp

cp /etc/modules /etc/modules.bkp

/bin/sed -i 's/#dtparam=i2s=on/dtparam=i2s=on/' /boot/config.txt

echo "snd-bcm2835" >> /etc/modules

wget https://raw.githubusercontent.com/notro/rpi-source/master/rpi-source -O /usr/bin/rpi-source

chmod +x /usr/bin/rpi-source

/usr/bin/rpi-source -q --tag-update

/usr/bin/rpi-source --skip-gcc --default-config

mount -t debugfs debugs /sys/kernel/debug

git clone https://github.com/garcezluz/rpi-i2s-audio

cd rpi-i2s-audio

make -C /lib/modules/$(uname -r )/build M=$(pwd) modules

insmod /root/rpi-i2s-audio/my_loader.ko >/dev/null 2>&1

cp /root/rpi-i2s-audio/my_loader.ko /lib/modules/$(uname -r)

echo 'my_loader' | tee --append /etc/modules > /dev/null

depmod -a >/dev/null 2>&1

modprobe my_loader >/dev/null 2>&1

