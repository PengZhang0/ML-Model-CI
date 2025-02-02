<p align="center"> <img src="docs/img/iconv1.svg" width="230" alt="..."> </p>

<h1 align="center">
    Machine Learning Model CI
</h1>

<p align="center">
    <a href="https://www.python.org/downloads/release/python-370/" title="python version"><img src="https://img.shields.io/badge/Python-3.7%2B-blue.svg"></a>
    <a href="https://github.com/cap-ntu/ML-Model-CI/actions" title="Build Status"><img src="https://github.com/cap-ntu/ML-Model-CI/actions/workflows/run_test.yml/badge.svg?branch=master"></a>
    <a href="https://app.fossa.com/projects/custom%2B8170%2Fgithub.com%2Fcap-ntu%2FML-Model-CI?ref=badge_shield" title="FOSSA Status"><img src="https://app.fossa.com/api/projects/custom%2B8170%2Fgithub.com%2Fcap-ntu%2FML-Model-CI.svg?type=shield"></a>
    <a href="https://www.codacy.com?utm_source=github.com&amp;utm_medium=referral&amp;utm_content=cap-ntu/ML-Model-CI&amp;utm_campaign=Badge_Grade" title="Codacy Badge"><img src="https://app.codacy.com/project/badge/Grade/bfb9f8b11d634602acd8b67484a43318"></a>
    <a href="https://codebeat.co/a/yizheng-huang/projects/github-com-cap-ntu-ml-model-ci-master"><img alt="codebeat badge" src="https://codebeat.co/badges/343cc340-21c6-4d34-ae2c-48a48e2862ba" /></a>
    <a href="https://github.com/cap-ntu/ML-Model-CI/graphs/commit-activity" title="Maintenance"><img src="https://img.shields.io/badge/Maintained%3F-YES-yellow.svg"></a>
    <a href="https://gitter.im/ML-Model-CI/community?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge" title="Gitter"><img src="https://badges.gitter.im/ML-Model-CI/community.svg"></a>
</p>

<p align="center">
    <a href="README_zh_CN.md">中文简介</a> •
    <a href="#introduction">Features</a> •
    <a href="#installation">Installation</a> •
    <a href="#quick-start">Quick Start</a> •
    <a href="#quickstart-with-notebook">Notebook</a> •
    <a href="#tutorial">Tutorial</a> •
    <a href="#contributing">Contributing</a> •
    <a href="#citation">Citation</a> •
    <a href="#license">License</a>
</p>

## Introduction
Machine Learning Model CI is a **one-stop machine learning MLOps platform on clouds**, aiming to solve the "last mile" problem between model training and model serving. We implement a highly automated pipeline between the trained models and the online machine learning applications.

We offer the following features and users 1) can register models to our system and enjoy the automated pipeline, 2) or use them individually.

- **Housekeeper** provides a refined management for model (service) registration, deletion, update and selection.
- **Converter** is designed to convert models to serialized and optimized formats so that the models can be deployed to cloud. Support **Tensorflow SavedModel**, **ONNX**, **TorchScript**, **TensorRT**
- **Profiler** simulates the real service behavior by invoking a gRPC client and a model service, and provides a 
    detailed report about model runtime performance (e.g. P99-latency and throughput) in production environment.
- **Dispatcher** launches a serving system to load a model in a containerized manner and dispatches the MLaaS to a device. Support **Tensorflow Serving**, **Trion Inference Serving**, **ONNX runtime**, **Web Framework (e.g., FastAPI)**
- **Controller** receives data from the monitor and node exporter, and controls the whole workflow of our system.

Several features are in beta testing and will be available in the next release soon. You are welcome to discuss them with us in the issues.

- [ ] **Automatic model quantization and pruning.** 
- [ ] **Model visulization and fine-tune.**

*The system is currently under rapid iterative development. Some APIs or CLIs may be broken. Please go to [Wiki](https://github.com/cap-ntu/ML-Model-CI/wiki) for more details*

*If your want to join in our development team, please contact huaizhen001 @ e.ntu.edu.sg*

## Installation

### using pip

```shell script
# upgrade installation packages first
pip install -U setuptools requests
# install modelci from GitHub
pip install git+https://github.com/cap-ntu/ML-Model-CI.git@master
```

### create conda workspace 

**Note**
- Conda and Docker are required to run this installation script.
- To use TensorRT, you have to manually install TensorRT (`sudo` is required). See instruction 
[here](https://docs.nvidia.com/deeplearning/tensorrt/install-guide/index.html). 

```shell script
bash scripts/install.sh
```

### Docker

![](https://img.shields.io/docker/pulls/mlmodelci/mlmodelci.svg) ![](https://img.shields.io/docker/image-size/mlmodelci/mlmodelci)

```shell script
docker pull mlmodelci/mlmodelci
```

<!-- Please refer to [here](/integration/README.md) for more information. -->


## Quick Start

The below figurs illusrates the 
| Web frontend |   Workflow     |
|:------------:|:--------------:|
| <img src="https://i.loli.net/2020/12/10/4FsfciXjtPO12BQ.png" alt="drawing" width="500"/> | <img src="https://i.loli.net/2020/12/10/8IaeW9mS2NjQEYB.png" alt="drawing" width="500"/>    |

### 0. Start the ModelCI service

Once you have installed, start MLModelCI service on a leader server by:
```shell script
modelci service init
```

**We provide three options for users to use MLModelCI: CLI, Web interface and import it as a python package**
### 1. CLI

```python
# publish a model to the system
modelci modelhub publish registration_example.yml
```

Please refer to [WIKI](https://github.com/cap-ntu/ML-Model-CI/wiki) for more CLIs.

### 2. Python Package

```python
# utilize the convert function
from modelci.hub.converter import ONNXConverter
from modelci.types.bo import IOShape

# the system can trigger the function automaticlly
# users can call the function individually 
ONNXConverter.from_torch_module(
    '<path to torch model>', 
    '<path to export onnx model>', 
    inputs=[IOShape([-1, 3, 224, 224], float)],
)
```

## Quickstart with Notebook

- [Publish an image classification model](./example/notebook/image_classification_model_deployment.ipynb)  [![nbviewer](https://raw.githubusercontent.com/jupyter/design/master/logos/Badges/nbviewer_badge.svg)](https://nbviewer.jupyter.org/github/cap-ntu/ML-Model-CI/blob/master/example/notebook/image_classification_model_deployment.ipynb)
- [Publish an object detection model](./example/notebook/object_detection_model_deployment.ipynb)  [![nbviewer](https://raw.githubusercontent.com/jupyter/design/master/logos/Badges/nbviewer_badge.svg)](https://nbviewer.jupyter.org/github/cap-ntu/ML-Model-CI/blob/master/example/notebook/object_detection_model_deployment.ipynb)
- [ ] Model performance analysis
- [ ] Model dispatch
- [ ] Model visualization&edit

## Tutorial

After the Quick Start, we provide detailed tutorials for users to understand our system.

- [Register a Model in ModelHub](./docs/tutorial/register.md)
- [Manage Models with Housekeeper within a Team](./docs/tutorial/housekeeper.md)
- [Convert a Model to Optimized Formats](./docs/tutorial/convert.md)
- [Profile a Model for Cost-Effective MLaaS](./docs/tutorial/profile.md)
- [Dispatch a Model as a Cloud Service](./docs/tutorial/retrieve-and-deploy.md)
- [ ] model visulization and editor within a team

## Contributing

MLModelCI welcomes your contributions! Please refer to [here](CONTRIBUTING.md) to get start.

## Citation

If you use MLModelCI in your work or use any functions published in MLModelCI, we would appreciate if you could cite:
```
@inproceedings{10.1145/3394171.3414535,
  author = {Zhang, Huaizheng and Li, Yuanming and Huang, Yizheng and Wen, Yonggang and Yin, Jianxiong and Guan, Kyle},
  title = {MLModelCI: An Automatic Cloud Platform for Efficient MLaaS},
  year = {2020},
  url = {https://doi.org/10.1145/3394171.3414535},
  doi = {10.1145/3394171.3414535},
  booktitle = {Proceedings of the 28th ACM International Conference on Multimedia},
  pages = {4453–4456},
  numpages = {4},
  location = {Seattle, WA, USA},
  series = {MM '20}
}
```

## Contact

Please feel free to contact our team if you meet any problem when using this source code. We are glad to upgrade the code meet to your requirements if it is reasonable.

We also open to collaboration based on this elementary system and research idea.

> *huaizhen001 AT e.ntu.edu.sg*

## License

```
   Copyright 2021 Nanyang Technological University, Singapore

   Licensed under the Apache License, Version 2.0 (the "License");
   you may not use this file except in compliance with the License.
   You may obtain a copy of the License at

       http://www.apache.org/licenses/LICENSE-2.0

   Unless required by applicable law or agreed to in writing, software
   distributed under the License is distributed on an "AS IS" BASIS,
   WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
   See the License for the specific language governing permissions and
   limitations under the License.
```
