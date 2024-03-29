.. SPDX-License-Identifier: GPL-2.0+

Multiple kernels, ramdisks and FDT blobs with Xen
=================================================

This example makes use of the 'loadables' field::

    /dts-v1/;

    / {
        description = "Configuration to load a Xen Kernel";
        #address-cells = <1>;

        images {
            xen_kernel {
                description = "xen binary";
                data = /incbin/("./xen");
                type = "kernel";
                arch = "arm";
                os = "linux";
                compression = "none";
                load = <0xa0000000>;
                entry = <0xa0000000>;
                hash-1 {
                    algo = "sha256";
                };
            };

            fdt-1 {
                description = "xexpress-ca15 tree blob";
                data = /incbin/("./vexpress-v2p-ca15-tc1.dtb");
                type = "flat_dt";
                arch = "arm";
                compression = "none";
                load = <0xb0000000>;
                hash-1 {
                    algo = "sha256";
                };
            };

            fdt-2 {
                description = "xexpress-ca15 tree blob";
                data = /incbin/("./vexpress-v2p-ca15-tc1.dtb");
                type = "flat_dt";
                arch = "arm";
                compression = "none";
                load = <0xb0400000>;
                hash-1 {
                    algo = "sha256";
                };
            };

            linux_kernel {
                description = "Linux Image";
                data = /incbin/("./Image");
                type = "kernel";
                arch = "arm";
                os = "linux";
                compression = "none";
                load = <0xa0000000>;
                entry = <0xa0000000>;
                hash-1 {
                    algo = "sha256";
                };
            };
        };

        configurations {
            default = "config-2";

            config-1 {
                description = "Just plain Linux";
                kernel = "linux_kernel";
                fdt = "fdt-1";
            };

            config-2 {
                description = "Xen one loadable";
                kernel = "xen_kernel";
                fdt = "fdt-1";
                loadables = "linux_kernel";
            };

            config-3 {
                description = "Xen two loadables";
                kernel = "xen_kernel";
                fdt = "fdt-1";
                loadables = "linux_kernel", "fdt-2";
            };
        };
    };
