.. SPDX-License-Identifier: GPL-2.0+

Signed Images
=============

::

    /dts-v1/;

    / {
        description = "Chrome OS kernel image with one or more FDT blobs";
        #address-cells = <1>;

        images {
            kernel {
                data = /incbin/("test-kernel.bin");
                type = "kernel_noload";
                arch = "sandbox";
                os = "linux";
                compression = "none";
                load = <0x4>;
                entry = <0x8>;
                kernel-version = <1>;
                signature {
                    algo = "sha256,rsa2048";
                    key-name-hint = "dev";
                };
            };
            fdt-1 {
                description = "snow";
                data = /incbin/("sandbox-kernel.dtb");
                type = "flat_dt";
                arch = "sandbox";
                compression = "none";
                fdt-version = <1>;
                signature {
                    algo = "sha256,rsa2048";
                    key-name-hint = "dev";
                };
            };
        };
        configurations {
            default = "conf-1";
            conf-1 {
                kernel = "kernel";
                fdt = "fdt-1";
            };
        };
    };
