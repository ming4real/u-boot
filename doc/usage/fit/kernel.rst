.. SPDX-License-Identifier: GPL-2.0+

Single kernel
=============

::

    /dts-v1/;

    / {
        description = "Simple image with single Linux kernel";
        #address-cells = <1>;

        images {
            kernel {
                description = "Vanilla Linux kernel";
                data = /incbin/("./vmlinux.bin.gz");
                type = "kernel";
                arch = "ppc";
                os = "linux";
                compression = "gzip";
                load = <00000000>;
                entry = <00000000>;
                hash-1 {
                    algo = "crc32";
                };
                hash-2 {
                    algo = "sha256";
                };
            };
        };

        configurations {
            default = "config-1";
            config-1 {
                description = "Boot Linux kernel";
                kernel = "kernel";
            };
        };
    };


For x86 a setup node is also required: see x86-fit-boot::

    /dts-v1/;

    / {
        description = "Simple image with single Linux kernel on x86";
        #address-cells = <1>;

        images {
            kernel {
                description = "Vanilla Linux kernel";
                data = /incbin/("./image.bin.lzo");
                type = "kernel";
                arch = "x86";
                os = "linux";
                compression = "lzo";
                load = <0x01000000>;
                entry = <0x00000000>;
                hash-2 {
                    algo = "sha256";
                };
            };

            setup {
                description = "Linux setup.bin";
                data = /incbin/("./setup.bin");
                type = "x86_setup";
                arch = "x86";
                os = "linux";
                compression = "none";
                load = <0x00090000>;
                entry = <0x00090000>;
                hash-2 {
                    algo = "sha256";
                };
            };
        };

        configurations {
            default = "config-1";
            config-1 {
                description = "Boot Linux kernel";
                kernel = "kernel";
                setup = "setup";
            };
        };
    };

Note: the above assumes a 32-bit kernel. To directly boot a 64-bit kernel,
change both arch values to "x86_64". U-Boot will then change to 64-bit mode
before booting the kernel (see boot_linux_kernel()).
