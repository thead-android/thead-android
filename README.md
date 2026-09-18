# LicheePi 4A Android 17

面向 TH1520 / C910 的 Android 17 开发版本。按 AOSP 多仓库形式组织：
改动已落实到各仓库的真实源码提交，不需要手动应用补丁包。

## 获取与构建

入口是 [local_manifests](https://github.com/thead-android/local_manifests)，
其中包含固定版本的 Android / kernel manifest、构建脚本及源码版本清单。

```sh
mkdir android17-lpi4a && cd android17-lpi4a
repo init -u https://github.com/thead-android/local_manifests -b android17
repo sync -c -j8
python3 prebuilts/thead/install.py --root "$PWD"
python3 vendor/thead/proprietary/prebuilts/generic/install-archived-apk.py
python3 .repo/manifests/tools/restore-large-assets.py --root "$PWD"
```

内核及 Android 的后续构建步骤见 manifest 仓库 README。内核、DTB、模块必须
来自同一构建，不能混入旧 5.10 模块。

## 关键仓库

- [C910 Clang / Rust 预编译工具链](https://github.com/thead-android/prebuilts-c910/releases/tag/v2026.09.18)
- [LLVM 源码](https://github.com/thead-android/toolchain-llvm-project/tree/android17)、[Rust 源码](https://github.com/thead-android/toolchain-rust/tree/android17)
- [板级配置](https://github.com/thead-android/device-thead-th1520/tree/android17)、[LPi4A 产品配置](https://github.com/thead-android/device-thead-th1520-lichee_pi_4a/tree/android17)
- [Linux 内核](https://github.com/thead-android/kernel-common/tree/android17)、[已有 U-Boot](https://github.com/thead-android/u-boot)
- [Mesa](https://github.com/thead-android/platform-external-mesa3d/tree/android17)、[Codec2 硬编接入](https://github.com/thead-android/platform-external-v4l2_codec2/tree/android17)

完整源码清单以 manifest / `source-lock.json` 为准。大于 GitHub 普通 Git 文件
限制的二进制放在校验过的 Release 附件中，不冒充源码，也不是补丁包。

这是可追溯的开发基线，不是全部 CTS 通过的稳定版。状态栏性能、部分 Wi-Fi
SAE/Mesh 互操作及长时间稳定性仍有待改进；不承诺 scrcpy 1080p60。
新发布仓库尚未完成一次从 GitHub 全新下载开始的完整编译、刷机验收。
请先阅读 manifest 仓库中的已知问题和刷写边界，不要直接覆盖 U-Boot 或其他槽。

保留上游版权与作者；本项目修改以 LoveSy <shana@zju.edu.cn> 提交。
