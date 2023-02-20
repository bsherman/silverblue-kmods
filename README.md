# ublue-kmods

[![build-ublue](https://github.com/bsherman/ublue-kmods/actions/workflows/build.yml/badge.svg)](https://github.com/bsherman/ublue-kmods/actions/workflows/build.yml)

A layer to build extra drivers into an image for consumption by other images.
Drivers included:
- xone
- xpadneo

If used directly, this image is a vanilla Silverblue plus the drivers listed above AND **nvidia** drivers as built in [ublue-os/nvidia](https://github.com/ublue-os/nvidia) (this image's base).

#### NOTE: this project is not formally affiliated with [ublue-os](https://github.com/ublue-os/) and is not supported by their team.


## Setup

1. Rebase onto the image

   Any system running `rpm-ostree` should be able to rebase onto one of the images built in this project:

       rpm-ostree rebase ostree-unverified-registry:ghcr.io/bsherman/ublue-kmods:latest

   And then reboot.

2. Set kargs after rebasing

```
rpm-ostree kargs \
    --append=rd.driver.blacklist=nouveau \
    --append=modprobe.blacklist=nouveau \
    --append=nvidia-drm.modeset=1
```
   And then reboot one more time!

3. Enable Secure Boot support

    [Secure Boot](https://rpmfusion.org/Howto/Secure%20Boot) support for the kernel modules can be enabled by enrolling the signing key:

```
# note there are two different keys, one for nvidia by ublue-os, one for custom kmods in this project
sudo mokutil --import /etc/pki/akmods/certs/akmods-custom.der
sudo mokutil --import /etc/pki/akmods/certs/akmods-nvidia.der
```


## Rolling back

   To rollback to a specific date, use a date tag:

       rpm-ostree rebase ostree-unverified-registry:ghcr.io/bsherman/ublue-kmods:20230220

 ## Verification

These images are signed with sigstore's [cosign](https://docs.sigstore.dev/cosign/overview/). You can verify the signature by downloading the `cosign.pub` key from this repo and running the following command:

    cosign verify --key cosign.pub ghcr.io/bsherman/ublue-kmods

## Other Details

Read more details about [building locally](https://github.com/ublue-os/nvidia#building-locally) and [using nvidia in containers](https://github.com/ublue-os/nvidia#using-nvidia-gpus-in-containers) in the [ublue-os/nvidia repo](https://github.com/ublue-os/nvidia).


## Acknowledgements

Thanks to Jorge Castro and [team ublue os](https://github.com/ublue-os) for their efforts to get people started with ostree native containers.
