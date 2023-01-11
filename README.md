![name](ugly.png)

## DIY 8-disks NAS based on Odroid H3+ under $1k with disks

### Why is it ugly?

Well to be honest it's not so ugly anymore. 

But early case versions were really [ugly](/ugly) and when I eventually created decent looking case I decided to leave its name.

### So how does it look now?

![nas1](IMG_0854.jpeg)
![nas2](IMG_0855.jpeg)
![nas3](IMG_0856.jpeg)
![nas4](IMG_0857.jpeg)

### Cost (November 2022):

| Part  | USD  |
|-------|--------:|
| [Odroid H3+](https://www.hardkernel.com/shop/odroid-h3-plus/) | 165.00 |
| [64GB eMMC Module](https://www.hardkernel.com/shop/64gb-emmc-module-h2/) | 39.90 |
| [Samsung 32GB DDR4](https://www.hardkernel.com/shop/samsung-32gb-ddr4-pc4-25600-so-dimm/) | 107.00 |
| [Case Type 5](https://www.hardkernel.com/shop/odroid-h3-case-type-5/) | 20.00 |
| [Power Supply Unit](https://www.hardkernel.com/shop/15v-4a-power-supply-asia-korea-plug-copy/) | 9.40 |
| 2 x [SATA/Power cables for disks](https://www.hardkernel.com/shop/sata-data-and-power-cable/) | 6.00 |
| Shipping from hardkernel.com to Poland | 39.43 |
| 8 x [Kioxia 2.5" SSD 480GB drives](https://www.ebay.com/itm/134327464843) | ~350.00 |
| [6-bays case](https://aliexpress.com/item/32921898033.html) | 48.29 |
| [Additional PSU for case](https://aliexpress.com/item/4000253348414.html) | 5.91 |
| [PCI-E SATA extender for 6 ports](https://aliexpress.com/item/1005004374186238.html) | 24.80 |
| 6 x [Profiled SATA cables](https://pl.aliexpress.com/item/1005002384391035.html) | 16.74 |
| [MOLEX splitter](https://aliexpress.com/item/1005004236892928.html) | 1.71 |
| **Sum** | **834.18** | 

### Performance

    root@nas[/mnt/storage]# /root/fio --randrepeat=1 --ioengine=libaio --direct=1 --gtod_reduce=1 --name=test --filename=test.fio --bs=4k --iodepth=64 --size=1G --readwrite=randrw --rwmixread=80
    test: (g=0): rw=randrw, bs=(R) 4096B-4096B, (W) 4096B-4096B, (T) 4096B-4096B, ioengine=libaio, iodepth=64
    fio-3.25
    Starting 1 process
    test: Laying out IO file (1 file / 1024MiB)
    Jobs: 1 (f=1): [m(1)][100.0%][r=134MiB/s,w=33.0MiB/s][r=34.3k,w=8701 IOPS][eta 00m:00s]
    test: (groupid=0, jobs=1): err= 0: pid=691249: Mon Dec  5 18:10:27 2022
      read: IOPS=22.9k, BW=89.6MiB/s (93.9MB/s)(819MiB/9142msec)
       bw (  KiB/s): min=21781, max=165208, per=99.06%, avg=90861.61, stdev=42994.49, samples=18
       iops        : min= 5445, max=41302, avg=22715.33, stdev=10748.69, samples=18
      write: IOPS=5744, BW=22.4MiB/s (23.5MB/s)(205MiB/9142msec); 0 zone resets
       bw (  KiB/s): min= 5343, max=41728, per=99.04%, avg=22756.61, stdev=10894.22, samples=18
       iops        : min= 1335, max=10432, avg=5689.06, stdev=2723.60, samples=18
      cpu          : usr=5.09%, sys=78.22%, ctx=5155, majf=4, minf=7
      IO depths    : 1=0.1%, 2=0.1%, 4=0.1%, 8=0.1%, 16=0.1%, 32=0.1%, >=64=100.0%
         submit    : 0=0.0%, 4=100.0%, 8=0.0%, 16=0.0%, 32=0.0%, 64=0.0%, >=64=0.0%
         complete  : 0=0.0%, 4=100.0%, 8=0.0%, 16=0.0%, 32=0.0%, 64=0.1%, >=64=0.0%
         issued rwts: total=209630,52514,0,0 short=0,0,0,0 dropped=0,0,0,0
         latency   : target=0, window=0, percentile=100.00%, depth=64

    Run status group 0 (all jobs):
       READ: bw=89.6MiB/s (93.9MB/s), 89.6MiB/s-89.6MiB/s (93.9MB/s-93.9MB/s), io=819MiB (859MB), run=9142-9142msec
      WRITE: bw=22.4MiB/s (23.5MB/s), 22.4MiB/s-22.4MiB/s (23.5MB/s-23.5MB/s), io=205MiB (215MB), run=9142-9142msec

And same test with 10M file instead of 1G:

    root@nas[/mnt/storage]# /root/fio --randrepeat=1 --ioengine=libaio --direct=1 --gtod_reduce=1 --name=test --filename=test.fio --bs=4k --iodepth=64 --size=10M --readwrite=randrw --rwmixread=80
    test: (g=0): rw=randrw, bs=(R) 4096B-4096B, (W) 4096B-4096B, (T) 4096B-4096B, ioengine=libaio, iodepth=64
    fio-3.25
    Starting 1 process

    test: (groupid=0, jobs=1): err= 0: pid=695373: Mon Dec  5 18:25:17 2022
      read: IOPS=97.1k, BW=379MiB/s (398MB/s)(8156KiB/21msec)
      write: IOPS=24.8k, BW=96.9MiB/s (102MB/s)(2084KiB/21msec); 0 zone resets
      cpu          : usr=0.00%, sys=90.00%, ctx=0, majf=0, minf=7
      IO depths    : 1=0.1%, 2=0.1%, 4=0.2%, 8=0.3%, 16=0.6%, 32=1.2%, >=64=97.5%
         submit    : 0=0.0%, 4=100.0%, 8=0.0%, 16=0.0%, 32=0.0%, 64=0.0%, >=64=0.0%
         complete  : 0=0.0%, 4=100.0%, 8=0.0%, 16=0.0%, 32=0.0%, 64=0.1%, >=64=0.0%
         issued rwts: total=2039,521,0,0 short=0,0,0,0 dropped=0,0,0,0
         latency   : target=0, window=0, percentile=100.00%, depth=64

    Run status group 0 (all jobs):
       READ: bw=379MiB/s (398MB/s), 379MiB/s-379MiB/s (398MB/s-398MB/s), io=8156KiB (8352kB), run=21-21msec
      WRITE: bw=96.9MiB/s (102MB/s), 96.9MiB/s-96.9MiB/s (102MB/s-102MB/s), io=2084KiB (2134kB), run=21-21msec

Feel free to ping me if you want me to run any other tests.

### Power consumption in Watts

| State | Peak | Avg |
|-------|-------:|-------:|
| standby | - | 3.6 |
| boot | 24.2 | ~17 |
| idling | 17.7 | ~14 |
| 1 VM running | 22.3 | ~16 |
| VM + stress CPU + fio test | 30.8 | ~28 |

Note: Don't try to use HDD instead of SDD with my power supply setup... it won't work almost for sure.

### Does it support ECC?

No. It can't.

### What software does it run?

It runs TrueNAS Scale
![truenas](truenas.png)

### Can it be any cheaper?

Yup.

Without disks it's below $500 ;)

Also it should still work pretty decent if you switch:
  * H3+ -> H3 - 36$ less
  * eMMC 64GB -> 32GB - $13 less
  * RAM 32GB -> 16GB - $54 less

So without disks it can be built for about $370.

### How to get that lovely case?

Once I figure out final version I will post all files needed to carve it with your local CNC dealer.

