# sbc-rockchip

This repo provides the overlay for RockChip based Talos image.

## Supported Overlay

| Overlay Name         | Board                 | Description                                   |
| -------------------- | --------------------- | --------------------------------------------- |
| nanopi-r4s           | NanoPi R4S            | Overlay for NanoPi R4S                        |
| orangepi-r1-plus-lts | Orange Pi R1 Plus LTS | Overlay for Orange Pi R1 Plus LTS             |
| rock64               | Pine64 Rock64         | Overlay for Pine64 Rock64                     |
| rockpi4              | Rock Pi 4A,Rock Pi 4B | Generic overlay for Rock Pi 4A and Rock Pi 4B |
| rockpi4c             | Rock Pi 4C            | Overlay for Rock Pi 4C                        |
| rock4cplus           | Radxa ROCK 4C+        | Overlay for Radxa ROCK 4C+                    |
| rock5b               | Radxa ROCK 5B         | Overlay for Radxa ROCK 5B                     |
| rock5a               | Radxa ROCK 5A         | Overlay for Radxa ROCK 5A                     |

## ROCK 5 WIP

*WARNING*: This repository will be rebased when needed

### Changes

- added rkbin for collabora u-boot build based on [pl4ntys PR](https://github.com/siderolabs/sbc-rockchip/pull/7)
- added rkbin alternative: collabora trusted-firmware-a (unused for now)
- added rock5b support
- added rock5a support (untested)

These changes use the [Rockchip RK3588 upstream enablement efforts](https://gitlab.collabora.com/hardware-enablement/rockchip-3588/notes-for-rockchip-3588) repositories.

### Current status

Builds fine using `make USERNAME=choopm PUSH=true`.

The resulting overlay can be tested using a custom installer built using:

```bash
docker run --rm -t -v $PWD/_out:/out -v /dev:/dev --privileged \
    ghcr.io/siderolabs/imager:v1.7.0 metal \
    --arch arm64 \
    --overlay-image ghcr.io/choopm/sbc-rockchip:$TAG \
    --overlay-name=rock5b
```

~~However booting the image resulted in *no network connection*.~~

~~The builtin r8169 driver will bring up the Rock 5B NIC which is a r8125.~~
~~A connected networking switch recognized the link as up, but no LEDs on the SBC were lit.~~

~~This probably happens since Talos ships kernel 6.6.29 at the time of this writing.~~
~~Support for PCIe of the Rock 5B is mainlined in 6.7-rc1.~~

~~In order to get this up and running we should move to a custom kernel until Talos switches to stable/next-longterm.~~

I built a custom [Talos arm64 kernel](https://github.com/choopm/pkgs-rock5) based on linux-stable: `ghcr.io/choopm/kernel:$TAG`

To include it in the image generation, use my custom imager:

```bash
docker run --rm -t -v $PWD/_out:/out -v /dev:/dev --privileged \
    ghcr.io/choopm/imager:$TAG metal \
    --arch arm64 \
    --overlay-image ghcr.io/choopm/sbc-rockchip:$TAG \
    --overlay-name=rock5b
```
