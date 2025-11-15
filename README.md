# Towards Low-Latency Event-based Obstacle Avoidance on a FPGA-Drone. 🔵

## [🛢 Dataset](https://zenodo.org/records/14711527) [📝 Paper](https://openaccess.thecvf.com/content/CVPR2025W/EventVision/html/Bonazzi_Towards_Low-Latency_Event-based_Obstacle_Avoidance_on_a_FPGA-Drone_CVPRW_2025_paper.html) [🎞️ Platform](https://github.com/ETH-PBL/SwiftEagle)

## References & Citation ✉️ 

Code base to reproduce results in :
``` 
@InProceedings{Bonazzi_2025_CVPR,
    author    = {Bonazzi, Pietro and Vogt, Christian and Jost, Michael and Khacef, Lyes and Paredes-Valles, Federico and Magno, Michele},
    title     = {Towards Low-Latency Event-based Obstacle Avoidance on a FPGA-Drone},
    booktitle = {Proceedings of the Computer Vision and Pattern Recognition Conference (CVPR) Workshops},
    month     = {June},
    year      = {2025},
    pages     = {4938-4946}
}

@InProceedings{Bonazzi_2025_IJCNN,
  author={Bonazzi, Pietro and Vogt, Christian and Jost, Michael and Qin, Haotong and Khacef, Lyes and Paredes-Valles, Federico and Magno, Michele},
  booktitle={2025 International Joint Conference on Neural Networks (IJCNN)}, 
  title={RGB-Event Fusion with Self-Attention for Collision Prediction}, 
  year={2025},
  pages={1-6},
  keywords={Event detection;Computational modeling;Neural networks;Robot vision systems;Predictive models;Throughput;Computational efficiency;Collision avoidance;Vehicle dynamics;Drones;Drone;TinyML;Obstacle Avoidance;Event-Based Camera},
  doi={10.1109/IJCNN64981.2025.11227211}}
```

Leave a star to support our open source initiative!⭐️ 

The name of the main folder with all the modules is `eva` which stands for Event Vision for Action prediction.

## 1. Installation instructions 🚀

### 1.1. Downloads 📥

To download the dataset click [here](https://zenodo.org/records/14711527).

Click on this [link](https://zenodo.org/records/15166553) to download the models.   

### 1.2.a. Conda env 🛠️

```
conda create -n eva-env python=3.10 -y
conda activate eva-env
pip install h5py numpy matplotlib opencv-python pandas scipy dtaidistance \
  pytorch-lightning torchvision torch fire wandb torchprofile onnx scikit-learn dotenv \
  pybind11 tensorboard tensorboardX
```

### 1.2.b. Docker containers 🛠️

Replace the path variable of the dataset (`ABCD_DATA_PATH`) to your installation directory. Build the docker container:
```
docker build -t eva-pytorch .
```

Run the docker container:
```
docker run --gpus all -it --rm \
  -v `ABCD_DATA_PATH`:/workspace/data \
  -e DISPLAY=$DISPLAY \
  --network host \
  --volume /tmp/.X11-unix:/tmp/.X11-unix \
  eva-pytorch
``` 

### 1.3. OpenEB for Event PreProcessing 🛠️

In your shell, inside the code directory, install [OpenEB](https://docs.prophesee.ai/stable/installation/linux_openeb.html), following these instructions :
```
git clone https://github.com/prophesee-ai/openeb.git --branch 4.6.0 
cd openeb && mkdir build && cd build 
cmake .. -DCMAKE_BUILD_TYPE=Release -DBUILD_TESTING=OFF 
cmake --build . --config Release -- -j $(nproc) 
. utils/scripts/setup_env.sh 
export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/usr/local/lib 
export HDF5_PLUGIN_PATH=$HDF5_PLUGIN_PATH:/usr/local/lib/hdf5/plugin 
export HDF5_PLUGIN_PATH=$HDF5_PLUGIN_PATH:/usr/local/hdf5/lib/plugin
cd ../..
``` 

## 2. Reproduce the results 🚀

First, visualize a recording 👀
```
python3 -m eva.data.subclasses.flight --id=1
``` 

Then, evaluate the EVS and RGB models 📚 For example Tab. 3 [1] can be reproduced as follows:
```
python3 -m scripts.eval.cvprw.tab_3
```

## 3. Train a model 🏋️‍♂️

To train the fusion model: 
```
python3 -m scripts.train --inputs_list=["dvs","rgb"] --name=fusion-model --frequency="rgb"
```

To train the event-based model: 
```
python3 -m scripts.train --inputs_list=["dvs"] --name=dvs-model --frequency="rgb"
```

To train the rgb-based model: 
```
python3 -m scripts.train --inputs_list=["rgb"] --name=rgb-model --frequency="rgb" 
```

## 4. Deploy the event-based model on AMD Kria K26  📦

Deploy instructions coming soon.
