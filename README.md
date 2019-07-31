# Ansible Role: fujitsu
富士通のLinuxインストールマニュアルに記載された、RHELの初期設定を行います。

## Sub Role: initial
カーネルパラメータやコアダンプ、GRUBの設定を変更します。
### 要件
無し
### 基本情報
- 依存ロール: 無し
- 冪等性: 有り
- 対応OS: RHEL7
- テスト環境: RHEL7.3
### 変数
このロールで設定すべき変数はありません。全て富士通の推奨する値が設定されます。

## Sub Role: fj_lsp
FJ-LSPを適用します。
### 要件
- ロール内のfilesディレクトリに、インストールするFJ-LSPのisoイメージファイルを格納してください。
- kernel-debuginfo及びkernel-debuginfo-commonパッケージをyumコマンドでインストールできる必要があります。
- RedHatの標準パッケージ群をインストールできるyumリポジトリを用意してください。
### 基本情報
- 依存ロール: 無し
- 冪等性: 有り
- 対応OS: RHEL7
- テスト環境: RHEL7.3
### 変数
- `FUJITSU_FJLSP_VERSION`
```yaml
# FJ-LSPのバージョンを指定します。
# ここで指定したバージョンのisoファイルをfiles/ディレクトリに格納してください。
FUJITSU_FJLSP_VERSION: UP1-1.0-0
```
- `FUJITSU_FJLSP_DEBUGINFO_VERSION`
```yaml
# インストールするkernel-debuginfoのバージョンを指定します。
FUJITSU_FJLSP_DEBUGINFO_VERSION: 3.10.0-229
```
- `FUJITSU_FJLSP_REPO`
```yaml
# FJ-LSPを適用するために必要なrpmをダウンロードするyumリポジトリを指定します。
# RHELメディアに含まれるRedHat標準パッケージが格納されたリポジトリを設定してください。
FUJITSU_FJLSP_REPO: http://example.com/repo
```

## Sub Role: kdump
富士通kdump設定ツール(FJSVdumptool)を利用し、kdumpの設定を行います。
### 要件
- FJSVdumptoolsがインストールされている必要があります。これはFJ-LSPによってインストールされます。
- 必要な容量のkdump取得先領域を用意してください。
- kdump取得先領域をリードオンリーでマウントするようfstabを設定してください。
### 基本情報
- 依存ロール: fujitsu/<OS名>/fj_lsp
- 冪等性: 有り
- 対応OS: RHEL7
- テスト環境: RHEL7.3
### 変数
- `FUJITSU_KDUMP_DISK`
```yaml
# kdumpを取得する領域のパスを設定します。
FUJITSU_KDUMP_DISK: /var/crash
```