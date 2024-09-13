## SETUP


# SERVER

### Triton Inference Server

'''bash
wget https://github.com/triton-inference-server/server/releases/download/v2.19.0/tritonserver2.19.0-jetpack4.6.1.tgz

tar -xvzf tritonserver2.19.0-jetpack4.6.1.tgz

rm tritonserver2.19.0-jetpack4.6.1.tgz

'''



# MODELS

'''bash
# Yolov10x 128288859 bytes 123.0 MB
wget https://github.com/THU-MIG/yolov10/releases/download/v1.1/yolov10x.pt
'''
# Convert Pytorch to TorchScript

'''
from ultralytics import YOLOv10
model = YOLOv10("model.pt")
model.export(format="torchscript") 
>>> Ultralytics YOLOv8.1.34 🚀 Python-3.8.10 torch-1.13.0a0+git7c98e70 CPU (ARMv8 Processor rev 1 (v8l))
>>> PyTorch: starting from 'model.pt' with input shape (1, 3, 640, 640) BCHW and output shape(s) (1, 300, 6) (122.3 MB)

'''bash
wget --content-disposition 'https://api.ngc.nvidia.com/v2/models/org/nvidia/team/tao/peoplenet/deployable_quantized_onnx_v2.6.2/files?redirect=true&path=resnet34_peoplenet.onnx' -O peoplenet.onnx

'''


### Triton Inference Server Client also in Triton Inference Server/clients

'''bash
wget https://github.com/triton-inference-server/server/releases/download/v2.19.0/v2.19.0_ubuntu2004.clients.tar.gz

tar -xvzf v2.19.0_ubuntu2004.clients.tar.gz

rm v2.19.0_ubuntu2004.clients.tar.gz

'''



./bin/tritonserver --model-repository=./models \
	--http-port=8000 \
	--grpc-port=8001 \
	--metrics-port=8002 \
	--allow-grpc=true \
	--allow-http=true \
	--allow-metrics=true \
	--allow-gpu-metrics=true \
	--log-verbose=1 \
	--strict-model-config=false \
	--strict-readiness=true \
	--exit-on-error=false \
	--backend-directory=./backends \
    --model-control-mode=explicit \
	--load-model=yolov10x,peoplenet \
