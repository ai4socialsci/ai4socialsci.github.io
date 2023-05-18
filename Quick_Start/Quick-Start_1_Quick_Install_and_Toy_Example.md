## Quick Install
We recommend to install the AI4SS project from the following codes
```shell
# download from the github 
git@github.com:alibaba-damo-academy/ai-for-social-science.git 

# install requirements
pip install requirements.txt


```
[Liuyi: pip install -r requirements.txt]<br />We also provide a detailed version of dependency from pip freeze if needed.

```shell
absl-py==1.4.0
aiosignal==1.3.1
attrs==22.2.0
cachetools==5.3.0
certifi @ file:///croot/certifi_1671487769961/work/certifi
charset-normalizer==3.1.0
click==8.1.3
cloudpickle==2.2.1
cycler==0.11.0
distlib==0.3.6
dm-tree==0.1.8
Farama-Notifications==0.0.4
filelock==3.10.7
fonttools==4.38.0
frozenlist==1.3.3
google-auth==2.17.1
google-auth-oauthlib==0.4.6
grpcio==1.53.0
gym==0.26.2
gym-notices==0.0.8
Gymnasium==0.26.3
gymnasium-notices==0.0.1
h5py==3.8.0
idna==3.4
imageio==2.27.0
importlib-metadata==6.1.0
importlib-resources==5.12.0
jax-jumpy==1.0.0
jsonschema==4.17.3
kiwisolver==1.4.4
llvmlite==0.39.1
lz4==4.3.2
Markdown==3.4.3
markdown-it-py==2.2.0
MarkupSafe==2.1.2
matplotlib==3.5.3
mdurl==0.1.2
msgpack==1.0.5
networkx==2.6.3
numba==0.56.4
numpy==1.21.6
nvidia-cublas-cu11==11.10.3.66
nvidia-cuda-nvrtc-cu11==11.7.99
nvidia-cuda-runtime-cu11==11.7.99
nvidia-cudnn-cu11==8.5.0.96
oauthlib==3.2.2
packaging==23.0
pandas==1.3.5
PettingZoo==1.22.3
Pillow==9.4.0
pkgutil_resolve_name==1.3.10
platformdirs==3.2.0
protobuf==3.20.3
pyasn1==0.4.8
pyasn1-modules==0.2.8
Pygments==2.14.0
pyparsing==3.0.9
pyrsistent==0.19.3
python-dateutil==2.8.2
pytz==2023.3
PyWavelets==1.3.0
PyYAML==6.0
ray==2.3.1
requests==2.28.2
requests-oauthlib==1.3.1
rich==13.3.3
rllib==0.0.1
rsa==4.9
scikit-image==0.19.3
scipy==1.7.3
seaborn==0.12.2
six==1.16.0
tabulate==0.9.0
tensorboard==2.11.2
tensorboard-data-server==0.6.1
tensorboard-plugin-wit==1.8.1
tensorboardX==2.6
tianshou==0.5.0
tifffile==2021.11.2
torch==1.13.1
tqdm==4.65.0
typer==0.7.0
typing_extensions==4.5.0
urllib3==1.26.15
virtualenv==20.21.0
Werkzeug==2.2.3
zipp==3.15.0
```

## Quick start for an auction 

To quick start a classic auction,  you can directly use the following command, where each agent by default performs a multi-arm bandit algorithm (epsilon-greedy) <br />[Liuyi: To quick start a classic second price auction]
```shell
# then you can directly take the examples from the scripts 
# the main file is the auction_bidding_simulate.py 
# the dynamic env file is the auction_bidding_simulate_multiple.py

python auction_bidding_simulate_multiple.py --mechanism 'second_price' --exp_id 33  --folder_name 'test' \
--bidding_range 10 --valuation_range 10 --env_iters 1000000 --overbid True \
--round 1 \
--estimate_frequent 100000 --revenue_averaged_stamp 1000 --exploration_epoch 100000 --player_num 5 \
--step_floor 10000 \
```


Then the auction results will saved in the folder "./results/test/33/'' with provided folder name "test" and experiment id "33". As denoted in the hyperparameter, the total round is 1, means the mechanism is fixed using "second price" and the item valuation range is 10.  <br />[Liuyi: what's other RL related hyper-parameters? e.g., revenue_averaged_stamp, step_floor?]

We display the final agent bidding policy results as below,  where the truthful bidding (bid/valuation =1) is the theoretical equilibrium result.<br />![image.png](https://intranetproxy.alipay.com/skylark/lark/0/2023/png/229273/1684208003453-c1358d2a-f2b5-45e1-8061-d894fa9f8e75.png?x-oss-process=image/format,png#clientId=u6aca483e-306e-4&from=paste&height=480&id=uf4398f10&originHeight=480&originWidth=640&originalType=binary&ratio=1&rotation=0&showTitle=false&size=1229090&status=done&style=none&taskId=ubf766edc-03da-4f02-a86b-05f1cc90144&title=&width=640)<br />Figure 1. The agent bidding policy results after 100k iterations.
