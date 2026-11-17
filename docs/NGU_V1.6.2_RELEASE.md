# FrankyRenderer 1.6.2

Status: Resolution-compatibility hotfix, 2026-11-17.

## Change

- Native NGU still requires its evidence-backed source geometries.
- When a valid NGU request is outside those geometries, Franky now uses a
  visible common-renderer fallback with `ewa_lanczossharp`, not bilinear.
- A 1920x800 SDR NV12 source is explicitly covered by the fallback regression.
- Version advanced to 1.6.2.

This does **not** claim that the native NGU graph itself supports arbitrary
source dimensions. The current NGU graph has source-bound dimensions and
constants; arbitrary NGU geometry remains a separate implementation milestone.

## Verification

```text
test_ngu_backend
NGU 1920x800 uses a visible EWA Lanczos Sharp common fallback
PASS_PRODUCT ngu_sharp backend RGB48 + P010 + class-3 -> 4K

packaged DirectShow
EXIT 0x0
```

PotPlayer loaded the packaged 1.6.2 AX and rendered the supplied 1920x800
file visibly. Its trace reports the expected NGU geometry rejection followed
by the visible common fallback.

## Artifacts

```text
ZIP
E:\FrankyRenderer\releases\local-packages-ngu-hdr-1.6.2-20261117-final\FrankyRenderer-1.6.2-x64.zip
6200D3D75054F57825B7E9C9CE406CE305C9DC550A1162E23E3F76067A58AF04

packaged AX
E:\FrankyRenderer\releases\local-packages-ngu-hdr-1.6.2-20261117-final\FrankyRenderer-1.6.2-x64\FrankyRenderer.ax
1A269E6D69AFDF0266890B13B244A8B0D643BD516922360C191285AB04D556E1

source AX
E:\FrankyRenderer\src\build\bin\FrankyRenderer.ax
B8D99051D5B45E7981CA50299A4424CEFC65EBAA2621BA8AFD3775EA6D9AF01F
```

```text
Backend log
65318D2A0C0286B7C04B70C411894626453066A5AC404F91415A5DC523F75CA3

Packaged DirectShow log
E44C9D10A381C75E92B25F60001DEC88B963130E49625797CCE88F570BB777AC

PotPlayer trace
AB91EC9CAA3E157828E139C6B2F8DF8D7CE71FEC1CF48043545A50516CDC563A
```

## Boundary

The 1920x800 file is SDR H.264 YUV420P, not a nonstandard HDR transfer. Its
source dimensions are valid; they are simply not one of the native NGU
geometry contracts. True arbitrary NGU requires a generic NGU graph, not a
new fallback alias.
