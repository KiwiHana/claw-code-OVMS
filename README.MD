Ubuntu24.04, kernel 大于等于6.17

首先验证环境，如果没有GPU设备，请安装相关驱动和固件：https://zhuanlan.zhihu.com/p/2032752671648658519
```
$ clinfo -l
Platform #0: Intel(R) OpenCL Graphics
 `-- Device #0: Intel(R) Arc(TM) B390 GPU
```

## 1. enable Qwen3.6-35B OVMS

下载Qwen3.6-35B-A3B-int4-ov到~/models，并
```
mv Qwen3.6-35B-A3B-int4-ov Qwen3.6-35B-A3B-ov
```
下载地址：https://huggingface.co/OpenVINO/Qwen3.6-35B-A3B-int4-ov ，或者 https://www.modelscope.cn/models/OpenVINO/Qwen3.6-35B-A3B-int4-ov

下载脚本并运行，https://github.com/KiwiHana/ragflow-OVMS/blob/main/models/run-35b-docker.sh
```
sudo chmod -R 777 run-35b-docker.sh
./run-35b-docker.sh
```


## 2. Clone and build
```
sudo apt install cargo
git clone https://github.com/ultraworkers/claw-code
cd claw-code/rust
cargo build --workspace
```

## 3. Start an interactive session
```
export OPENAI_API_KEY="unused"
export OPENAI_BASE_URL="http://127.0.0.1:8000/v3"

./target/debug/claw  --model Qwen3.6-35B-A3B-ov
```

## 已知issue
```
~/claw-code/rust$ cargo build --workspace
error: failed to parse lock file at: /home/intel/claw-code/rust/Cargo.lock

Caused by:
  lock file version 4 requires `-Znext-lockfile-bump`

$ cargo --version
cargo 1.75.0
```

#### 解决方案：
#### 1. 先卸载系统自带的 rust（避免冲突）

```
sudo apt remove rustc cargo
sudo apt autoremove
```

#### 2. 安装 rustup

```
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
```

执行过程默认回车选 **1) Proceed with installation (default)**

#### 3. 加载环境变量

```
source "$HOME/.cargo/env"
```

#### 4. 验证 & 升级到最新 stable

```
rustup update stable
cargo --version
```
cargo 1.97.1 (c980f4866 2026-06-30)

此时版本会 ≥1.78，就能正常解析 v4 Cargo.lock

然后回到项目编译：

```
cd ~/claw-code/rust
cargo build --workspace
```

