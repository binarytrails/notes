# drive

## wipe

* Method 1: filling with zeros

        dd if=/dev/zero of=/dev/sdX2 bs=1M #replace X with the target drive letter.

* Method 2: filling with random data

> In practice using either will work similay only a modern drive (with caveats below) however using /dev/urandom is slower and safer. Slower because it needs to build entropy, safer because it prevents the (practical on very old drives) attack of amplifying the read signal to recover and differentiate a 1 from a 0. The reality is data is spaced so close together on modern drives the zero amplification attack can not work because drive tolerances are to tight and close to theoretical limits.

        dd if=/dev/urandom of=/dev/sdX3 bs=1M

> A more realistic issue with both solutions is that DD may not write parts of the drive marked bad and thus fragments of data might be recoverable.
