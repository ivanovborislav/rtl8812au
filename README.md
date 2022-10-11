-----

## rtl8812au (8812au.ko)

## Realtek RTL8812AU Wireless Lan Driver for Linux

- v5.13.6 (20210629)
- Based on EDIMAX EW-7822UAC Linux Driver (Version : 1.0.4.1) 2021-09-17
- Support Kernel: 4.4 - 5.11 (Realtek)
- Support up to Kernel 6.0.0

##	Added

- Support for Raspberry Pi
- LED control
- Devices, which is compatible with this driver

## Enable

- Monitor mode (resolves a `fixed channel 1` issue.)
- Packet injection
- REGD source selection

## HOW TO

### Install

Download source:

```
git clone https://github.com/ivanovborislav/rtl8812au.git
cd rtl8812au
```

Install missing packages:

```
sudo apt-get install bc build-essential
```

Install linux headers:

```
sudo apt-get install linux-headers-$(uname -r)
```

or

```
apt-cache search linux-headers
sudo apt-get install linux-headers-5.14.0-kali4-amd64 (for example)
apt-cache search linux-image
sudo apt-get install linux-image-5.14.0-kali4-amd64 (for example)
```

Compile:

```
make
sudo make install
```

Raspberry Pi:

Edit `Makefile`:

Ln141 - CONFIG_PLATFORM_I386_PC = `y` to CONFIG_PLATFORM_I386_PC = `n`

Ln142 - CONFIG_PLATFORM_RPI_ARM = `n` to CONFIG_PLATFORM_RPI_ARM = `y` for ARM

or

Ln143 - CONFIG_PLATFORM_RPI_ARM64 = `n` to CONFIG_PLATFORM_RPI_ARM64 = `y` for ARM64

### Monitor mode

```
sudo airmon-ng check kill
sudo ip link set <interface> down
sudo iw dev <interface> set type monitor
sudo ip link set <interface> up
```

### Managed mode

```
sudo ip link set <interface> down
sudo iw dev <interface> set type managed
sudo ip link set <interface> up
sudo systemctl restart NetworkManager (sudo service network-manager restart)
```

### TX power control

Note: Set TX power before start SoftAP mode. `...set txpower fixed 3000 = txpower 30.00 dBm`.

```
sudo iw dev <interface> set txpower fixed 3000
```

### Driver options

#### Change driver options during inserting driver module

Remove (unload) a module from the Linux kernel.
```
sudo rmmod /lib/modules/$(uname -r)/kernel/drivers/net/wireless/8812au.ko
```

Insert (load) a module into the Linux kernel.
```
sudo insmod /lib/modules/$(uname -r)/kernel/drivers/net/wireless/8812au.ko rtw_ips_mode=1 rtw_drv_log_level=4 rtw_power_mgnt=2 rtw_led_ctrl=1
```

#### Change driver options loading from file

Create a file `8812au.conf` containing `options 8812au rtw_ips_mode=1 rtw_drv_log_level=4 rtw_power_mgnt=2 rtw_led_ctrl=1`.
Copy a file to `/etc/modprobe.d/` directory.

```
sudo cp -f 8812au.conf /etc/modprobe.d
```

Power saving control.

IPS (Inactive Power Saving) Function, rtw_ips_mode=
```
0:Disable IPS
1:Enable IPS (default)
```

LPS (Leisure Power Saving) Function, rtw_power_mgnt=
```
0:Disable LPS
1:Enable LPS
2:Enable LPS with clock gating (default)
```

Driver debug log level control, rtw_drv_log_level=
```
0:_DRV_NONE_
1:_DRV_ALWAYS_
2:_DRV_ERR_
3:_DRV_WARNING_
4:_DRV_INFO_ (default)
5:_DRV_DEBUG_
6:_DRV_MAX_
```

Driver LED control, rtw_led_ctrl=
```
0:led off
1:led blink (default)
2:led on
```

Driver VHT control, rtw_vht_enable=
```
0:disable
1:enable (default)
2:force auto enable (allow width: 80 MHz, AP mode)
```

Driver wireless mode control, rtw_wireless_mode=
```
1: 2.4GHz 802.11b
2: 2.4GHz 802.11g
3: 2.4GHz 802.11b/g
4: 5GHz 802.11a
8: 2.4Hz 802.11n
11: 2.4GHz 802.11b/g/n
16: 5GHz 802.11n
20: 5GHz 802.11a/n
64: 5GHz 802.11ac
84: 5GHz 802.11a/n/ac
95: 2.4GHz 802.11b/g/n 5GHz 802.11a/n/ac (default)
```

Driver REGD source selection, rtw_regd_src=
```
0:Realtek defined
1:OS (default, get channel plan from OS)
```

### [Add support rtl8821AU to rtl8812AU v5.13.6 driver (support chipset RTL8812AU, RTL8821AU, RTL8811AU)](https://github.com/ivanovborislav/document/blob/main/add_support_rtl8821AU_to_rtl8812AU_v5.13.6_driver.md)

-----
