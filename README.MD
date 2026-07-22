Ubuntu24.04

## 1. enable Qwen3.6-35B OVMS

https://github.com/KiwiHana/ragflow-OVMS/blob/main/models/run-35b-docker.sh
```
run run-35b-docker.sh
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
