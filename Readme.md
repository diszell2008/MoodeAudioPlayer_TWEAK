So, now i did some more tests and i am trying to put all information about sound quality tweaking in one place.

1. This is a must - wifi off, bt off, hdmi off, volume control off, eq off, resampling off.

2. added in /boot/config.txt :

force_turbo=1
avoid_pwm_pll=1
arm_freq=333
over_voltage=-7
core_freq=166
h264_freq=166
isp_freq=166
v3d_freq=166
gpu_freq=166
sdram_freq=166
sdram_schmoo=0x021e882c
over_voltage_sdram_p=5
over_voltage_sdram_i=3
over_voltage_sdram_c=-13
gpu_mem=16


3. added in /boot/cmdline.txt:

dwc_otg.microframe_schedule=0 dle=0 dwc_otg.nak_holdoff_enable=1 dwc_otg.fiq_fix_enable=0 dwc_otg.lpm_enable=0 smsc95xx.turbo_mode=N isolcpus=2,3

isolcpus is isolating two cpus only for mpd and works only on quadcore pi's

4. changed options in /lib/systemd/system/mpd.service, original value is uncommented:

#CPUSchedulingPolicy=other
CPUSchedulingPolicy=fifo

tried different, for me fifo sounds best.

#CPUSchedulingPriority=43
CPUSchedulingPriority=70

tried higher settings, but 70 is a good compromise

#Nice=-10
Nice=-15

also tried higher values, but -15 is a good compromise

#ExecStart=/usr/local/bin/mpd --no-daemon $MPDCONF
ExecStart=/usr/bin/taskset -c 2,3 /usr/local/bin/mpd --no-daemon $MPDCONF

this one is starting mpd using isolated cpus and no other processes are using those cpus.

My streaming is sounding as good, as never before and now it is clear for me, that underclocking cpus and ram is really great.

Thanks to anybody, who found out, how to "audiophilise" and "de-digitituslise" moOde

Maybe some of those tweaks (which are not making system unstable) can be implemented in moOde?
