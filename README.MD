Ubuntu24.04

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
