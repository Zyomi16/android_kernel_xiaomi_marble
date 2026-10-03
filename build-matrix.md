# Yomi16 build matrix — 20 variants for marble (SM7475)

Base: `marble_defconfig` (monolithic, 1152 lines) + fragments in
`arch/arm64/configs/vendor/`. Toolchain: ClangBuiltLinux 23.1.1 pinned,
`ARCH=arm64 LLVM=1 LLVM_IAS=1`. Kernel 5.10.270.

Fragment files (all verified with `merge_config.sh` + `olddefconfig` locally):

| File | Effect |
|---|---|
| `yomi16_bbr2.fragment` | BBR2 + DEFAULT_BBR2 + FQ qdisc + Full LTO |
| `yomi16_bbr2_thin.fragment` | BBR2 + DEFAULT_BBR2 + FQ qdisc, keeps ThinLTO |
| `yomi16_susfs.fragment` | ReSukiSU + multi-manager + SUSFS (9 opts, no SPOOF_UNAME) |
| `yomi16_resukisu.fragment` | ReSukiSU + multi-manager, **no** SUSFS |
| `yomi16_sukisu.fragment` | SukiSU Ultra, no KPM |
| `yomi16_sukisu_kpm.fragment` | `CONFIG_KPM=y` overlay for sukisu |
| `yomi16_lto_none.fragment` | LTO off entirely |

## Variants

| # | Name | Fragments (after defconfig) | KCFLAGS | Expected artifact |
|---|---|---|---|---|
| 1 | plain-thin-O3 | (none) | -O3 | Image, stock ThinLTO, Westwood default |
| 2 | plain-full-O3 | yomi16_lto_full* | -O3 | Image, Full LTO, Westwood |
| 3 | plain-none-O3 | yomi16_lto_none | -O3 | Image, no LTO, Westwood |
| 4 | plain-thin-O2 | (none) | -O2 | Image, ThinLTO, -O2 |
| 5 | bbr2-thin-O3 | yomi16_bbr2_thin | -O3 | Image, BBR2 default, ThinLTO |
| 6 | bbr2-full-O3 | yomi16_bbr2 | -O3 | Image, BBR2 default, Full LTO |
| 7 | bbr2-none-O3 | yomi16_bbr2_thin + yomi16_lto_none | -O3 | Image, BBR2, no LTO |
| 8 | bbr2-thin-O2 | yomi16_bbr2_thin | -O2 | Image, BBR2, ThinLTO, -O2 |
| 9 | bbr2-full-susfs | yomi16_bbr2 + yomi16_susfs | -O3 | Image, BBR2 + ReSukiSU + SUSFS |
| 10 | bbr2-thin-susfs | yomi16_bbr2_thin + yomi16_susfs | -O3 | Image, BBR2 + ReSukiSU + SUSFS, ThinLTO |
| 11 | bbr2-full-resukisu | yomi16_bbr2 + yomi16_resukisu | -O3 | Image, BBR2 + ReSukiSU, no SUSFS |
| 12 | bbr2-thin-sukisu | yomi16_bbr2_thin + yomi16_sukisu | -O3 | Image, BBR2 + SukiSU, ThinLTO |
| 13 | bbr2-full-sukisu | yomi16_bbr2 + yomi16_sukisu | -O3 | Image, BBR2 + SukiSU, Full LTO |
| 14 | bbr2-thin-sukisu-kpm | yomi16_bbr2_thin + yomi16_sukisu + yomi16_sukisu_kpm | -O3 | Image, BBR2 + SukiSU + KPM |
| 15 | bbr2-full-sukisu-kpm | yomi16_bbr2 + yomi16_sukisu + yomi16_sukisu_kpm | -O3 | Image, BBR2 + SukiSU + KPM, Full LTO |
| 16 | plain-full-susfs | yomi16_lto_full* + yomi16_susfs | -O3 | Image, Westwood + ReSukiSU + SUSFS |
| 17 | plain-thin-sukisu | yomi16_sukisu | -O3 | Image, Westwood + SukiSU, ThinLTO |
| 18 | bbr2-full-O2 | yomi16_bbr2 | -O2 | Image, BBR2, Full LTO, -O2 |
| 19 | plain-thin-susfs | yomi16_susfs | -O3 | Image, Westwood + ReSukiSU + SUSFS, ThinLTO |
| 20 | bbr2-none-sukisu-kpm | yomi16_bbr2_thin + yomi16_lto_none + yomi16_sukisu + yomi16_sukisu_kpm | -O3 | Image, BBR2 + SukiSU + KPM, no LTO |

\* `yomi16_lto_full.fragment` = `CONFIG_LTO_CLANG_FULL=y` +
`# CONFIG_LTO_CLANG_THIN is not set` (split out of yomi16_bbr2 so plain
variants can use Full LTO without BBR2). To be created before the matrix run.

Status: `pending` for all 20. Updated per run below.

## Run log

(none yet)
