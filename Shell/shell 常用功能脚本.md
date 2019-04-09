# shell 常用功能脚本



## 网络相关

1.ping测试脚本

```shell
#!/bin/sh
IP="8.8.8.8"
ping_status() {
    if ping -c 1 $IP > /dev/null; then
    	echo "$IP Ping is successful."
    	return 0                      
    else                                  
    	echo "$IP Ping is failure"
    	return 1                       
    fi                                     
}                             
ping_status
echo $? 
```





## 外设相关

1.操作GPIO复位

```shell

#!/bin/sh

# This script is intended to be used on IoT Starter Kit platform, it performs
# the following actions:
#       - export/unpexort GPIO7 used to reset the SX1301 chip
#
# Usage examples:
#       ./reset_lgw.sh stop
#       ./reset_lgw.sh start

# The reset pin of SX1301 is wired with MT7628 GPIO11
# If used on another platform, the GPIO number can be given as parameter.
if [ -z "$2" ]; then
    IOT_SK_SX1301_RESET_PIN=11
else
    IOT_SK_SX1301_RESET_PIN=$2
fi

echo "Accessing concentrator reset pin through GPIO$IOT_SK_SX1301_RESET_PIN..."

WAIT_GPIO() {
    sleep 1
}

iot_sk_init() {
    # setup GPIO 11
    echo "$IOT_SK_SX1301_RESET_PIN" > /sys/class/gpio/export; WAIT_GPIO

    # set GPIO 11 as output
    echo "out" > /sys/class/gpio/gpio$IOT_SK_SX1301_RESET_PIN/direction; WAIT_GPIO

    # write output for SX1301 reset
    echo "1" > /sys/class/gpio/gpio$IOT_SK_SX1301_RESET_PIN/value; WAIT_GPIO
    echo "0" > /sys/class/gpio/gpio$IOT_SK_SX1301_RESET_PIN/value; WAIT_GPIO

    # set GPIO 11 as input
    echo "in" > /sys/class/gpio/gpio$IOT_SK_SX1301_RESET_PIN/direction; WAIT_GPIO
}

iot_sk_term() {
    # cleanup GPIO 11
    if [ -d /sys/class/gpio/gpio$IOT_SK_SX1301_RESET_PIN ]
    then
        echo "$IOT_SK_SX1301_RESET_PIN" > /sys/class/gpio/unexport; WAIT_GPIO
    fi
}

case "$1" in
    start)
    iot_sk_term
    iot_sk_init
    ;;
    stop)
    iot_sk_term
    ;;
    *)
    echo "Usage: $0 {start|stop} [<gpio number>]"
    exit 1
    ;;
esac

exit 0
```





