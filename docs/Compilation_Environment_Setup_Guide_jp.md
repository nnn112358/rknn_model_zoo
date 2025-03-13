# コンパイル環境セットアップガイド

x86 Linuxシステム上でこのプロジェクトの例にあるC/C++ Demoをコンパイルする前に、クロスコンパイル環境をセットアップする必要があります。


## Androidプラットフォーム

ターゲットデバイスが`Android`システムの場合、ルートディレクトリにある`build-android.sh`スクリプトを使用して、特定のモデルのC/C++ Demoをコンパイルします。  
このスクリプトを使用してC/C++ Demoをコンパイルする前に、環境変数`ANDROID_NDK_PATH`を通じてAndroid NDKのパスを指定してください。

### Android NDKのダウンロード

*(システムにAndroid NDKがすでにインストールされている場合は、このステップを無視してください)*

1. 次のリンクからNDKをダウンロードしてください（r19cバージョンをダウンロードすることをお勧めします）：https://dl.google.com/android/repository/android-ndk-r19c-linux-x86_64.zip
2. ダウンロードしたAndroid NDKを解凍します。この解凍後のパスを覚えておいてください。後でC/C++ Demoをコンパイルするときに使用します。**注意: 上記NDKの解凍後のディレクトリ名は`android-ndk-r19c`です。**

### C/C++ Demoのコンパイル

C/C++ Demoをコンパイルするコマンドは以下の通りです：
```shell
export ANDROID_NDK_PATH=<android_ndk_path>

./build-android.sh -t <TARGET_PLATFORM> -a <ARCH> -d <model_name>
# RK3588の場合:
./build-android.sh -t rk3588 -a arm64-v8a -d mobilenet
# RK3566の場合:
./build-android.sh -t rk3566 -a arm64-v8a -d mobilenet
# RK3568の場合:
./build-android.sh -t rk3568 -a arm64-v8a -d mobilenet
```
*説明:*
- `<android_ndk_path>`: Android NDKのパスを指定します。例：`~/opt/android-ndk-r19c`。
- `<TARGET_PLATFORM>`: ターゲットプラットフォームを指定します。例：`rk3566`、`rk3568`、`rk3588`。**注意: `RK1808`、`RV1109`、`RV1126`、`RV1103`、`RV1106`は`Android`プラットフォームをサポートしていません。**
- `<ARCH>`: システムアーキテクチャを指定します。システムアーキテクチャを確認するには、以下のコマンドを参照してください：
	```shell
	# アーキテクチャを確認。Androidの場合、ログに['arm64-v8a'または'armeabi-v7a']が表示されるはずです。
	adb shell cat /proc/version
	```
- `model_name`: モデル名。examplesディレクトリ内の各モデルのフォルダ名です。


## Linuxプラットフォーム

ターゲットデバイスが`Linux`システムの場合、ルートディレクトリにある`build-linux.sh`スクリプトを使用して、特定のモデルのC/C++ Demoをコンパイルします。  
このスクリプトを使用してC/C++ Demoをコンパイルする前に、環境変数`GCC_COMPILER`を通じてクロスコンパイルツールのパスを指定してください。

### クロスコンパイルツールのダウンロード

*(システムにクロスコンパイルツールがすでにインストールされている場合は、このステップを無視してください)*

1. 異なるシステムアーキテクチャは異なるクロスコンパイルツールに依存しています。以下は、異なるシステムアーキテクチャに推奨されるクロスコンパイルツールのダウンロードリンクです：
   - aarch64: https://releases.linaro.org/components/toolchain/binaries/6.3-2017.05/aarch64-linux-gnu/gcc-linaro-6.3.1-2017.05-x86_64_aarch64-linux-gnu.tar.xz
   - armhf: https://developer.arm.com/-/media/Files/downloads/gnu-a/8.3-2019.03/binrel/gcc-arm-8.3-2019.03-x86_64-arm-linux-gnueabihf.tar.xz?revision=e09a1c45-0ed3-4a8e-b06b-db3978fd8d56&rev=e09a1c450ed34a8eb06bdb3978fd8d56&hash=9C4F2E8255CB4D87EABF5769A2E65733
   - armhf-uclibcgnueabihf(RV1103/RV1106)：https://console.zbox.filez.com/l/H1fV9a（取得コード：rknn）
2. ダウンロードしたクロスコンパイルツールを解凍し、そのパスを覚えておいてください。後でコンパイル時に使用します。

### C/C++ Demoのコンパイル

C/C++ Demoをコンパイルするコマンドの参考は以下の通りです：
```shell
# rknn_model_zooのルートディレクトリに移動
cd <rknn_model_zoo_root_path>

# ビルド中にGCC_COMPILERが見つからない場合は、GCC_COMPILERのパスを設定してください
export GCC_COMPILER=<GCC_COMPILER_PATH>

./build-linux.sh -t <TARGET_PLATFORM> -a <ARCH> -d <model_name>

# RK3588の場合
./build-linux.sh -t rk3588 -a aarch64 -d mobilenet
# RK3566の場合
./build-linux.sh -t rk3566 -a aarch64 -d mobilenet
# RK3568の場合
./build-linux.sh -t rk3568 -a aarch64 -d mobilenet
# RK1808の場合
./build-linux.sh -t rk1808 -a aarch64 -d mobilenet
# RV1109の場合
./build-linux.sh -t rv1109 -a armhf -d mobilenet
# RV1126の場合
./build-linux.sh -t rv1126 -a armhf -d mobilenet
# RV1103の場合
./build-linux.sh -t rv1103 -a armhf -d mobilenet
# RV1106の場合
./build-linux.sh -t rv1106 -a armhf -d mobilenet
```

*説明:*
- `<GCC_COMPILER_PATH>`: クロスコンパイルのパスを指定します。異なるシステムアーキテクチャでは異なるクロスコンパイルツールを使用します。
    - `GCC_COMPILE_PATH`の例：
        - aarch64: ~/tools/cross_compiler/arm/gcc-linaro-6.3.1-2017.05-x86_64_aarch64-linux-gnu/bin/aarch64-linux-gnu
        - armhf: ~/tools/cross_compiler/arm/gcc-arm-8.3-2019.03-x86_64-arm-linux-gnueabihf/bin/arm-linux-gnueabihf
        - armhf-uclibcgnueabihf(RV1103/RV1106): ~/tools/cross_compiler/arm/arm-rockchip830-linux-uclibcgnueabihf/bin/arm-rockchip830-linux-uclibcgnueabihf
- `<TARGET_PLATFORM>`: ターゲットプラットフォームを指定します。例：`rk3588`。**注意：現在各モデルでサポートされているターゲットプラットフォームは異なる場合があります。詳細は特定のモデルディレクトリ内の`README.md`ドキュメントを参照してください。**
- `<ARCH>`: システムアーキテクチャを指定します。システムアーキテクチャを確認するには、以下のコマンドを参照してください：
  ```shell
  # アーキテクチャを確認。Linuxの場合、ログに['aarch64'または'armhf']が表示されるはずです。
  adb shell cat /proc/version
  ```
- `model_name`: モデル名。examplesディレクトリ内の各モデルのフォルダ名です。
