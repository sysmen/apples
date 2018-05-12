1. apply the patch 3.19.8-APPLES.patch to linux kernel version 3.19.8

2. remember to pin VCPU on PCPU one by one before you run the workload in VM for stable performance

3. To enable the APLE component, use the following command:
$ echo -n 1 > /sys/module/core/parameters/aple_enable

4. To disable APLE component, use the following command:
$ echo -n 0 > /sys/module/core/parameters/aple_enable

5. To enable HVS component, use the following command:
$ echo -n 1 > /sys/module/kvm/parameters/mple_enable

6. To disable HVS component, use the following command:
$ echo -n 0 > /sys/module/kvm/parameters/mple_enable
7. 

Some tunable parameters:

a) ple_delta 

It describes how ple_window is changed across epochs. The default value is 20, meaning the window would be changed by 20%. For example, if the initial value is 100000, APLE would increase it to 100000+100000*0.2 or decrease it to 100000+100000*0.2 if adjustments are needed. 


This parameter can be adjusted with the following command
$ echo -n <value> > /sys/module/core/parameters/ple_delta


b) ple_min_delta 

It describes the minimum change of the ple_window. The default value is 1024. For example, if the initial value of ple_window is 4000 and ple_delta is 20, because 0.2*4000=800 is smaller than ple_min_delta, APLE would change the ple_windo value based on ple_min_delta to 4000+1024 or 4000-1024.


This parameter can be adjusted with command

$ echo -n <value> > /sys/module/core/parameters/ple_min_delta


c) ple_max_window 

It describes the maximum value of ple_window. The default value is 1000000. The value can be increased up to INT_MAX.


This parameter can be adjusted with command

$ echo -n <value> > /sys/module/core/parameters/ple_max_window


d) ple_min_window 

It describes the minimum value of ple_window. The default value is 4096. Usually this parameter has smaller impact on performance than other parameters when the value is not greater than 4096. Thus, there is little need to change it.


This parameter can be adjusted with command

$ echo -n <value> > /sys/module/core/parameters/ple_min_window


e) ple_counter 

It describes the duration of an epoch. The default value is 1000, meaning that each epoch contains 1000 time of ple_exit events. This parameter should be large enough to ensure the accurate measurement of the overhead for adjustment.

This parameter can be adjusted with command

$ echo -n <value> > /sys/module/core/parameters/ple_counter
