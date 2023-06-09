## Theoritcal equilibrium in Second Price
### Basic setting
We follow the basic setting in our auction simulated environment from [1], which consists of the following components with sealed second price mechanism (Vickery Auction[2]).

- Independent bidders  ![](https://intranetproxy.alipay.com/skylark/lark/__latex/4787ef6775bbb255dcfeea378956293f.svg#card=math&code=i%3D1%2C2..N&id=pECGU)
- Sold items ![](https://intranetproxy.alipay.com/skylark/lark/__latex/78696e84e463662798d8a1dd910ca0e9.svg#card=math&code=j%3D1%2C2..M&id=PfZ0j)
- Independent observed private signal (or valuation)  : ![](https://intranetproxy.alipay.com/skylark/lark/__latex/366696689be40c5e5185beaf02a9308f.svg#card=math&code=S_i%20%5Csim%20F_i%28.%29&id=PbJQ4), with typical realization ![](https://intranetproxy.alipay.com/skylark/lark/__latex/18f310cb9e895ee1182602ba6ca5b9c9.svg#card=math&code=s_i%20%5Cin%5Bs_%7Bmin%7D%2Cs_%7Bmax%7D%5D&id=s4yWM) and assume ![](https://intranetproxy.alipay.com/skylark/lark/__latex/fee5b77b0296d77a9f9b01af5821beb2.svg#card=math&code=F_i&id=ngaSv) is continous 
- Bidder ![](https://intranetproxy.alipay.com/skylark/lark/__latex/2443fbcfeb7e85e1d62b6f5e4f27207e.svg#card=math&code=i&id=S1WKr)'s value is ![](https://intranetproxy.alipay.com/skylark/lark/__latex/559a1ee0f185e637bd82b7bdf87e8fe4.svg#card=math&code=v_i%28s_i%29%20%3D%20s_i&id=iBDFL), which is independent of other bidder's value and information. 


### Vickery auction equilibrium results
**Proposition** 1: In a second price auction with independent private value, it's a weakly dominant strategy to bid one's value, ![](https://intranetproxy.alipay.com/skylark/lark/__latex/0b95626152c47ea32e62385aa4e3f02a.svg#card=math&code=b_i%28s_i%29%20%3D%20v_i%3Ds_i&id=l8s70)

## Experiment equilibrium validation  

We apply multi-agent methodology to simulate the auction game and compute their best response after their bidding policies converged. We apply no-regret algorithm using **Epsilon-greedy**, **MWU**, and **DQN** to validate the results. The convergence guarantee has been proved in [3,4].

The envirionment and algorithm learning hyperparameter of all the experiment is listed below, <br /> 

| **Overall Parameter** | **Explanation** |
| --- | --- |
| `args.mechanism = "first_price"` | Auction mechanism: first price sealed auction |
| `args.allocation_mode = "highest"` | Allocation mode: highest bidder win |
| `args.overbid` | Whether bidder is allowed to bid more than its value |
| `args.bidding_range` | The range of bidder's bidding value is [0, bidding_range) |
| `args.folder_name` | Plotted figures during the training steps will be saved in './results/{folder_name}/' |
| `args.env_iters ` | Number of iterations in one round (i.e. 'env.step()' in the code) |
| `args.estimate_frequent` | Estimate policy results per {estimate_frequent} iters |
| `args.revenue_averaged_stamp ` | Platform revenue is record and averaged per {revenue_averaged_stamp} iters |
| `args.exploration_epoch` | Number of exploration iters for agent's algorithm |
| `args.step_floor` | epsilon decay parameter |
| `args.buffer_size ` | buffer size in deep rl algorithm |
| `args.batchsize` | batch size for deep rl algorithm |
| `args.lr` | learning rate |


### Setting 1. Symmetric i.i.d Uniform distribution  

#### Experiment setting 
##### exp1-1 :  2 bidders in sealed SP auction 
We first validate the results in the **Proposition-1  **with 2 bidders with discrete valuation sampled from Uniform distribution ![](https://intranetproxy.alipay.com/skylark/lark/__latex/19fa58097401a8abe4bb347163258d84.svg#card=math&code=F_i%3DU%5B0%2C10%5D&id=al8tN) where we apply smart agent to simulate the bidder's behaviors and explore his optmial bidding policy. <br />Three algorithm is applied, including epsilon-greedy, DQN and MWU algorithm. <br />For different algorithms, we set:

- `args.env_iters = 2e6, args.exploration_epoch = 2e5` for Epsilon-greedy;
- `args.env_iters = 4e6, args.exploration_epoch = 2e6` for DQN;
- `args.env_iters = 1e5` for MWU.
##### exp1-2 : ablation study : bidders 
We then increase bidder number to validate its effects. We choose agent 0 as an example to display whether his policy will sharply changed when bidder number increase.<br />Three algorithm is applied, including epsilon-greedy, DQN and MWU algorithm. <br />For different algorithms, we set:

- `args.env_iters = 2e6, args.exploration_epoch = 2e5` for Epsilon-greedy;
- `args.env_iters = 4e6, args.exploration_epoch = 2e6` for DQN;
- `args.env_iters = 1e5` for MWU.
##### exp1-3 : ablation study : valuation range  
We then show the results when we change the valuation range from [0,10] to [0,20], [0,50] and [0,100] with 4 bidders. <br />Three algorithm is applied, including epsilon-greedy, DQN and MWU algorithm. <br />For different algorithms, we set:

- `args.env_iters = 2e6, args.exploration_epoch = 2e5` for Epsilon-greedy;
- `args.env_iters = 4e6, args.exploration_epoch = 2e6` for DQN;
- `args.env_iters = 1e5/2e5/5e5/1e6` for MWU when valuation range is 10/20/50/100.
##### hyper parameters

- in the DQN  algorithm, we set `args.batchsize = 128 , args.lr = 5e-4`
- in the MWU algorithm, we set `args.lr = 0.05 `
#### runnable script
We record the runnable script in the "./Equilibrium_Explore/script/Explore_1_SP/", and we identify the scripts using different algorithms with the filename postfix: "run_sp_exp1.sh", "run_sp_exp1_dqn.sh" or "run_sp_exp1_mwu.sh". And here we use "run_sp_exp1.sh" which uses default epsilon-greedy algorithm as an example:

```python
# EXP1 -- Symmetric i.i.d Uniform distribution
# exp 1-1
python3 Explore_1_SP.py --mode="exp_1" --valuation_range=10 --player_num=2 --exp_id=11 &&
# exp 1-2: increasing player num
python3 Explore_1_SP.py --mode="exp_1" --valuation_range=10 --player_num=3 --exp_id=123 &&
python3 Explore_1_SP.py --mode="exp_1" --valuation_range=10 --player_num=4 --exp_id=124 &&
python3 Explore_1_SP.py --mode="exp_1" --valuation_range=10 --player_num=5 --exp_id=125 &&
python3 Explore_1_SP.py --mode="exp_1" --valuation_range=10 --player_num=6 --exp_id=126 &&
# exp 1-3: different valuation range
python3 Explore_1_SP.py --mode="exp_1" --valuation_range=10 --player_num=4 --exp_id=131 &&
python3 Explore_1_SP.py --mode="exp_1" --valuation_range=20 --player_num=4 --exp_id=132 &&
python3 Explore_1_SP.py --mode="exp_1" --valuation_range=50 --player_num=4 --exp_id=135 &&
python3 Explore_1_SP.py --mode="exp_1" --valuation_range=100 --player_num=4 --exp_id=1310 &&
```

We also illustrate the core hyperparameters as below, 

| **Core Parameter** | **Explanation** |
| --- | --- |
| `mode` | Different experiment mode |
| `valuation_range` | Valuation_range is the upper_bound of bidder's value.<br />In exp1, bidder's valuation is sampled from Uniform[0, valuation_range) |
| `player_num` | Number of bidders |
| `exp_id` | Result saving id |
| `algorithm` | DQN or MWU or default algorithm(epsilon-greedy) |
| `gpu` | GPU id for training deep rl algorithm |


#### Theoritical results
From the theoritcal results, the ratio of the bidding price and the value shoud become 1.

#### Experiment results
##### exp1-1 :  2 bidders in sealed SP auction 
Bidder's bid(v)/v considering different valuation v:<br />![exp_1_valuation_10_player_num_2.png](https://intranetproxy.alipay.com/skylark/lark/0/2023/png/94556578/1685171903219-9589ffec-30ab-49af-adfc-a8d51d99221d.png#clientId=ua17b3828-073a-4&from=drop&id=u99b2dc61&originHeight=480&originWidth=640&originalType=binary&ratio=2&rotation=0&showTitle=false&size=19889&status=done&style=none&taskId=ub1bad432-fb07-4ea4-a57d-64a6c106557&title=)<br />(a) Epsilon-greedy<br />![exp_1_valuation_10_player_num_2.png](https://intranetproxy.alipay.com/skylark/lark/0/2023/png/94556578/1688016892242-13a7f1be-d909-4a09-8b09-d83b426689cb.png#clientId=ueb9e8aed-7604-4&from=paste&height=240&id=u0185e623&originHeight=480&originWidth=640&originalType=binary&ratio=2&rotation=0&showTitle=false&size=29025&status=done&style=none&taskId=uee65cdc1-898c-49f5-ac35-30a88aa1d6a&title=&width=320)<br />(b) DQN<br />![exp_1_valuation_10_player_num_2.png](https://intranetproxy.alipay.com/skylark/lark/0/2023/png/94556578/1687944226553-1b0d9892-7528-430c-a750-4da702800089.png#clientId=u59950d31-e8f0-4&from=paste&height=240&id=u85d6f73b&originHeight=480&originWidth=640&originalType=binary&ratio=2&rotation=0&showTitle=false&size=17130&status=done&style=none&taskId=u4a01b819-83d3-4070-bb48-295f0900225&title=&width=320)<br />(c) MWU<br />Fig1.1 Second price simulated results with 2 bidders valuation: U[0,10) [exp1-1]
##### exp1-2 : ablation study : bidders 
Agent 0's bidding policy (bid/valuation) when player num= 2/3/4/5/6:<br />![exp1-2.png](https://intranetproxy.alipay.com/skylark/lark/0/2023/png/94556578/1685172179115-0ed6c4db-eb3b-40e5-8508-b84fb320e615.png#clientId=ua17b3828-073a-4&from=drop&id=uac26af56&originHeight=480&originWidth=640&originalType=binary&ratio=2&rotation=0&showTitle=false&size=31076&status=done&style=none&taskId=u93dbf6a4-21f0-41b2-b35a-9b6a63a8656&title=)<br />(a) Epsilon-greedy<br />![1-2.png](https://intranetproxy.alipay.com/skylark/lark/0/2023/png/94556578/1688017980610-4bec677f-9a3c-4b65-85d6-06e8a5cb3845.png#clientId=ueb9e8aed-7604-4&from=paste&height=240&id=u38039611&originHeight=480&originWidth=640&originalType=binary&ratio=2&rotation=0&showTitle=false&size=46254&status=done&style=none&taskId=ucd4e70ae-24bb-45ad-a53b-24a01108be1&title=&width=320)<br />(b) DQN<br />![1-2.png](https://intranetproxy.alipay.com/skylark/lark/0/2023/png/94556578/1687946188646-8969eafe-ba16-4cc3-bfee-1727cadb1631.png#clientId=u31a90f0f-b048-4&from=paste&height=240&id=u1c78605f&originHeight=480&originWidth=640&originalType=binary&ratio=2&rotation=0&showTitle=false&size=28107&status=done&style=none&taskId=udd89c08b-6996-4fbe-a1c1-2d563c7b00a&title=&width=320)<br />(c) MWU<br />Fig 1.2 Second price simulated results with 2~6 bidders valuation: U[0,10) [exp1-2]

##### exp1-3 : ablation study : valuation range  
4 bidders' bidding policy, when the valuation range is [0,10):<br />![exp_1_valuation_10_player_num_4.png](https://intranetproxy.alipay.com/skylark/lark/0/2023/png/94556578/1685172188203-727a65ac-b726-4476-aae7-7a4efbe7f254.png#clientId=ua17b3828-073a-4&from=drop&id=u72735f11&originHeight=480&originWidth=640&originalType=binary&ratio=2&rotation=0&showTitle=false&size=19899&status=done&style=none&taskId=ubc79325b-b328-4478-9ac7-5812ef48f22&title=)<br />(a) Epsilon-greedy<br />![exp_1_valuation_10_player_num_4.png](https://intranetproxy.alipay.com/skylark/lark/0/2023/png/94556578/1688016909320-a4e57fa6-6725-4dbd-aa55-e017f6d7da7b.png#clientId=ueb9e8aed-7604-4&from=paste&height=240&id=u636447c4&originHeight=480&originWidth=640&originalType=binary&ratio=2&rotation=0&showTitle=false&size=38692&status=done&style=none&taskId=u356470af-797d-4481-82f7-07e0fdcd5b7&title=&width=320)<br />(b) DQN<br />![exp_1_valuation_10_player_num_4.png](https://intranetproxy.alipay.com/skylark/lark/0/2023/png/94556578/1687941880172-c49922c3-5d6c-4b8c-929f-65f4196722a1.png#clientId=ubdbb7c40-54fb-4&from=paste&height=240&id=u6e498724&originHeight=480&originWidth=640&originalType=binary&ratio=2&rotation=0&showTitle=false&size=19899&status=done&style=none&taskId=u1f89745e-b790-48e9-a63a-9f7bb8a688c&title=&width=320)<br />(c) MWU<br />Fig 1.3.1 Second price simulated results with 4 bidders valuation: U[0,10) [exp1-3]

4 bidders' bidding policy, when the valuation range is [0,20):<br />![exp_1_valuation_20_player_num_4.png](https://intranetproxy.alipay.com/skylark/lark/0/2023/png/94556578/1685172195426-5faf72b0-8aa2-473c-8484-903fbc41117a.png#clientId=ua17b3828-073a-4&from=drop&id=ue9aaaeee&originHeight=480&originWidth=640&originalType=binary&ratio=2&rotation=0&showTitle=false&size=40659&status=done&style=none&taskId=uf2248772-ee19-417c-877c-40283a6652e&title=)<br />(a) Epsilon-greedy<br />![exp_1_valuation_20_player_num_4.png](https://intranetproxy.alipay.com/skylark/lark/0/2023/png/94556578/1688016917265-a251015b-ba2c-4c5d-9011-e9d629f3fb18.png#clientId=ueb9e8aed-7604-4&from=paste&height=240&id=u68737b3f&originHeight=480&originWidth=640&originalType=binary&ratio=2&rotation=0&showTitle=false&size=43913&status=done&style=none&taskId=u5b58e03a-8f73-4417-bc20-74852b72df5&title=&width=320)<br />(b) DQN<br />![exp_1_valuation_20_player_num_4.png](https://intranetproxy.alipay.com/skylark/lark/0/2023/png/94556578/1687945984870-d945985c-b015-4491-b0ad-f793d23af6b5.png#clientId=u31a90f0f-b048-4&from=paste&height=240&id=u06f9fb4b&originHeight=480&originWidth=640&originalType=binary&ratio=2&rotation=0&showTitle=false&size=25824&status=done&style=none&taskId=u567a9f46-2fae-47f5-903e-c3c71eea5bd&title=&width=320)<br />(c) MWU<br />Fig 1.3.2 Second price simulated results with 4 bidders valuation: U[0,20) [exp1-3]

4 bidders' bidding policy, when the valuation range is [0,50):<br />![exp_1_valuation_50_player_num_4.png](https://intranetproxy.alipay.com/skylark/lark/0/2023/png/94556578/1685172218986-58c9082a-45c0-4c44-8fe9-71673f5545d2.png#clientId=ua17b3828-073a-4&from=drop&id=u82215b51&originHeight=480&originWidth=640&originalType=binary&ratio=2&rotation=0&showTitle=false&size=61215&status=done&style=none&taskId=u1a85fb5f-efb7-48c3-8f08-d7cf72bd4bd&title=)<br />(a) Epsilon-greedy<br />![exp_1_valuation_50_player_num_4.png](https://intranetproxy.alipay.com/skylark/lark/0/2023/png/94556578/1688016932866-7cbd5aba-8c8b-4a8a-ba13-31f484e0b859.png#clientId=ueb9e8aed-7604-4&from=paste&height=240&id=ua48eb6cf&originHeight=480&originWidth=640&originalType=binary&ratio=2&rotation=0&showTitle=false&size=55225&status=done&style=none&taskId=u69df7a37-744a-40a0-a38d-b2eccc11b50&title=&width=320)<br />(b) DQN<br />![exp_1_valuation_50_player_num_4.png](https://intranetproxy.alipay.com/skylark/lark/0/2023/png/94556578/1687945992062-dd238e17-f200-46a1-814b-e41f788841d0.png#clientId=u31a90f0f-b048-4&from=paste&height=240&id=ube37cf8c&originHeight=480&originWidth=640&originalType=binary&ratio=2&rotation=0&showTitle=false&size=26051&status=done&style=none&taskId=u4a29dfa6-6b67-4182-9ade-dbaee4ae578&title=&width=320)<br />(c) MWU<br />Fig 1.3.3 Second price simulated results with 4 bidders valuation: U[0,50) [exp1-3]<br />4 bidders' bidding policy, when the valuation range is [0,100):<br />![exp_1_valuation_100_player_num_4.png](https://intranetproxy.alipay.com/skylark/lark/0/2023/png/94556578/1685172225721-3adc75ee-2f55-4190-bb76-7b65d529dbf8.png#clientId=ua17b3828-073a-4&from=drop&id=u5f8cedca&originHeight=480&originWidth=640&originalType=binary&ratio=2&rotation=0&showTitle=false&size=72482&status=done&style=none&taskId=u1dcacf64-0e88-43a0-81a7-d0a42d3f381&title=)<br />(a) Epsilon-greedy<br />![exp_1_valuation_100_player_num_4.png](https://intranetproxy.alipay.com/skylark/lark/0/2023/png/94556578/1688381051437-71516c56-d4d7-4fde-afaf-7471a71282fe.png#clientId=uab3a92e7-eaad-4&from=paste&height=240&id=u303f7b10&originHeight=480&originWidth=640&originalType=binary&ratio=2&rotation=0&showTitle=false&size=57916&status=done&style=none&taskId=u7ddb8007-a785-45ae-bb60-3aeda8d8ea1&title=&width=320)<br />(b) DQN (batchsize32 lr1e-4)

![exp_1_valuation_100_player_num_4.png](https://intranetproxy.alipay.com/skylark/lark/0/2023/png/94556578/1688015040403-2f87760a-f747-490a-8eb5-bc9d27d1dbf7.png#clientId=u960ba388-6074-4&from=paste&height=240&id=ud5f66632&originHeight=480&originWidth=640&originalType=binary&ratio=2&rotation=0&showTitle=false&size=28068&status=done&style=none&taskId=u841dba92-78ef-40a0-bd00-f52758755ed&title=&width=320)<br />(c) MWU<br />Fig 1.3.4 Second price simulated results with 4 bidders valuation: U[0,100) [exp1-3]
#### Conclusion
##### exp1-1 :  2 bidders in sealed SP auction 
As we can see, the value of bid(v)/v of both player 0 and player 1 converge to around 1, which is in line with the theory result.
##### exp1-2 : ablation study : bidders 
Although the number of bidders has increased, the value of bid(v)/v still converges to around 1.
##### exp1-3 : ablation study : valuation range  
Although the valuation range has increased, the value of bid(v)/v still converges to around 1.
### Setting 2. Symmetric i.i.d  Gaussian distribution  
#### Experiment setting 
##### exp2 :  4 bidders in sealed SP auction 
Here we consider different valuation distribution's impact on bidder's policy. We validate the results** **with 4 bidders with discrete valuation sampled from Gaussian distribution ![](https://intranetproxy.alipay.com/skylark/lark/__latex/cc79e457fdbd3b1baa704204b42e5745.svg#card=math&code=F_i%3D%5Cmathcal%20N%2850%2C16%29&id=tlbNc) where we apply smart agent to simulate the bidder's behaviors and explore his optmial bidding policy. <br />Three algorithm is applied, including epsilon-greedy, DQN and MWU algorithm. <br />For different algorithms, we set:

- `args.env_iters = 2e6, args.exploration_epoch = 2e5` for Epsilon-greedy;
- `args.env_iters = 4e6, args.exploration_epoch = 2e6` for DQN;
- `args.env_iters = 1e6` for MWU.
##### hyper parameters

- in the DQN  algorithm, we set `args.batchsize = 128 , args.lr = 5e-4`
- in the MWU algorithm, we set `args.lr = 0.05 `
#### runnable script
We record the runnable script in the "./Equilibrium_Explore/script/Explore_1_SP/run_sp_exp1.sh",
```python
# EXP2 -- Symmetric i.i.d  Gaussian distribution
python3 Explore_1_SP.py --mode="exp_2" --valuation_range=100 --player_num=4 --exp_id=2 &&
```
We also illustrate the core hyperparameters as below, 

| **Core Parameter** | **Explanation** |
| --- | --- |
| `mode` | Different experiment mode |
| `valuation_range` | Valuation_range is the upper_bound of bidder's value.<br />In exp1, bidder's valuation is sampled from Uniform[0, valuation_range) |
| `player_num` | Number of bidders |
| `exp_id` | Result saving id |
| `algorithm` | DQN or MWU or default algorithm(epsilon-greedy) |
| `gpu` | GPU id for training deep rl algorithm |


#### Theoritical results
From the theoritcal results, the ratio of the bidding price and the value shoud become 1.

#### Experiment results
4 bidders' bid(v)/v considering different valuation v:<br />![exp_2_valuation_100_player_num_4.png](https://intranetproxy.alipay.com/skylark/lark/0/2023/png/94556578/1685172237817-cdb3d86b-b3ce-4f20-bf3e-49412c0a1339.png#clientId=ua17b3828-073a-4&from=drop&id=u38933151&originHeight=480&originWidth=640&originalType=binary&ratio=2&rotation=0&showTitle=false&size=78191&status=done&style=none&taskId=uab7a4a02-c749-442d-a5c5-95e9fe66492&title=)<br />(a) Epsilon-greedy<br />![exp_2_valuation_100_player_num_4.png](https://intranetproxy.alipay.com/skylark/lark/0/2023/png/94556578/1688016991319-c2e68387-5651-4de0-9044-4bd0859373fc.png#clientId=ueb9e8aed-7604-4&from=paste&height=240&id=u4a3c84cc&originHeight=480&originWidth=640&originalType=binary&ratio=2&rotation=0&showTitle=false&size=45812&status=done&style=none&taskId=uad1c8243-9274-43da-87ca-a128674d57d&title=&width=320)<br />(b) DQN<br />![exp_2_valuation_100_player_num_4.png](https://intranetproxy.alipay.com/skylark/lark/0/2023/png/94556578/1688015052614-5c09e95b-e9bb-4b27-8be8-a27bc2bd6ec4.png#clientId=u960ba388-6074-4&from=paste&height=240&id=ud7d767f8&originHeight=480&originWidth=640&originalType=binary&ratio=2&rotation=0&showTitle=false&size=33501&status=done&style=none&taskId=ufdde3f52-6917-4fc2-8f07-fbd47d8fb92&title=&width=320)<br />(c) MWU<br />Fig2.1 Second price simulated results with 4 bidders Gaussian distribution's valuation [exp2]
#### Conclusion
As we can see in Fig2.1, the trend of bid(v)/v value is consistent with the theoretical prediction.


### Setting 3. Asymmetric i.i.d Uniform distribution  
#### Experiment setting 
##### exp3-1 :  2 bidders in sealed SP auction 
We first validate the results in the **Proposition-1  **with 2 bidders with discrete valuation sampled from Uniform distribution ![](https://intranetproxy.alipay.com/skylark/lark/__latex/319b5cf1c3bf046f019301bfb956649a.svg#card=math&code=F_1%3DU%5B0%2C20%5D%2C%20F_2%3DU%5B5%2C15%5D&id=oWmXO) , with the same expected value ![](https://intranetproxy.alipay.com/skylark/lark/__latex/4c111d9cf81bf2a912dd2d1395ca5b33.svg#card=math&code=E%28F_1%29%3DE%28F_2%29&id=HwhkH)where we apply smart agent to simulate the bidder's behaviors and explore his optmial bidding policy. <br />Three algorithm is applied, including epsilon-greedy, DQN and MWU algorithm. <br />For different algorithms, we set:

- `args.env_iters = 2e6, args.exploration_epoch = 2e5` for Epsilon-greedy;
- `args.env_iters = 4e6, args.exploration_epoch = 2e6` for DQN;
- `args.env_iters = 2e5` for MWU.
##### exp3-2 : ablation study : bidders&valuation range
We then increase bidder number to validate its effects. The valuation distrbution is listed below,

- 2 bidders : ![](https://intranetproxy.alipay.com/skylark/lark/__latex/319b5cf1c3bf046f019301bfb956649a.svg#card=math&code=F_1%3DU%5B0%2C20%5D%2C%20F_2%3DU%5B5%2C15%5D&id=eSext) 
- 3 bidders : ![](https://intranetproxy.alipay.com/skylark/lark/__latex/bc8c45462b4492b6970e21596da5d9bd.svg#card=math&code=F_1%3DU%5B0%2C20%5D%2C%20F_2%3DU%5B5%2C15%5D%20%2C%20F_3%20%3D%20%5B8%2C12%5D&id=jxeF6)
- 5 bidders :   ![](https://intranetproxy.alipay.com/skylark/lark/__latex/8072ed1cdb6c89027089d562419b5494.svg#card=math&code=F_1%3DF_4%3DU%5B0%2C20%5D%2C%20F_2%3DF_5%3DU%5B5%2C15%5D%20%2C%20F_3%20%3D%20%5B8%2C12%5D&id=qvQam)

We choose agent 0 as an example to display whether his policy will sharply changed when bidder number increase.<br />Three algorithm is applied, including epsilon-greedy, DQN and MWU algorithm. <br />For different algorithms, we set:

- `args.env_iters = 2e6, args.exploration_epoch = 2e5` for Epsilon-greedy;
- `args.env_iters = 4e6, args.exploration_epoch = 2e6` for DQN;
- `args.env_iters = 2e5` for MWU.
##### hyper parameters

- in the DQN  algorithm, we set `args.batchsize = 128 , args.lr = 5e-4`
- in the MWU algorithm, we set `args.lr = 0.05 `
#### runnable script
We record the runnable script in the "./Equilibrium_Explore/script/Explore_1_SP/run_sp_exp1.sh",
```python
# EXP3 -- Asymmetric i.i.d Uniform distribution
# 2 players' valuation: [0,20], [5,15]
python3 Explore_1_SP.py --mode="exp_3" --valuation_range=21 --player_num=2 --exp_id=32 &&
# 3 players' valuation: [0,20], [5,15], [8,12]
python3 Explore_1_SP.py --mode="exp_3" --valuation_range=21 --player_num=3 --exp_id=33 &&
# 5 players' valuation: [0,20], [5,15], [8,12], [5,15], [0,20]
python3 Explore_1_SP.py --mode="exp_3" --valuation_range=21 --player_num=5 --exp_id=35
```
We also illustrate the core hyperparameters as below, 

| **Core Parameter** | **Explanation** |
| --- | --- |
| `mode` | Different experiment mode |
| `valuation_range` | Valuation_range is the upper_bound of bidder's value.<br />In exp1, bidder's valuation is sampled from Uniform[0, valuation_range) |
| `player_num` | Number of bidders |
| `exp_id` | Result saving id |
| `algorithm` | DQN or MWU or default algorithm(epsilon-greedy) |
| `gpu` | GPU id for training deep rl algorithm |


#### Theoritical results
From the theoritcal results, the ratio of the bidding price and the value shoud become 1.

#### Experiment results
##### exp3-1 :  2 bidders in sealed SP auction 
 2 bidders' bid(v)/v considering different valuation v:<br />![exp_3_valuation_21_player_num_2.png](https://intranetproxy.alipay.com/skylark/lark/0/2023/png/94556578/1685172276452-be497696-7e7a-4ad9-a743-7f1c2142e594.png#clientId=ua17b3828-073a-4&from=drop&id=ue8d147f6&originHeight=480&originWidth=640&originalType=binary&ratio=2&rotation=0&showTitle=false&size=32468&status=done&style=none&taskId=uc8a89ece-0c04-461f-ace9-4947fb7501f&title=)<br />(a) Epsilon-greedy<br />![exp_3_valuation_21_player_num_2.png](https://intranetproxy.alipay.com/skylark/lark/0/2023/png/94556578/1688017011210-284abea4-00e1-4e6c-8962-aec5f03bee1b.png#clientId=ueb9e8aed-7604-4&from=paste&height=240&id=ue48c7582&originHeight=480&originWidth=640&originalType=binary&ratio=2&rotation=0&showTitle=false&size=31947&status=done&style=none&taskId=u16fc3260-ced5-42b6-b742-465fe27b52c&title=&width=320)<br />(b) DQN<br />![exp_3_valuation_21_player_num_2.png](https://intranetproxy.alipay.com/skylark/lark/0/2023/png/94556578/1688015070505-ea07323c-eedb-47f4-9333-43ad702eceab.png#clientId=u960ba388-6074-4&from=paste&height=240&id=u679d8b55&originHeight=480&originWidth=640&originalType=binary&ratio=2&rotation=0&showTitle=false&size=18672&status=done&style=none&taskId=u4e46b8d4-780a-4be1-9776-cec7ef994bd&title=&width=320)<br />(c) MWU<br />Fig3.1 Second price simulated results with 2 bidders with asymmetric valuation [exp3-1]
##### exp3-2 : ablation study : bidders&valuation range
Agent 0's bidding policy (bid/valuation) under different number of bidders:<br />![exp3-2.png](https://intranetproxy.alipay.com/skylark/lark/0/2023/png/94556578/1685172898280-76efa63f-bea4-48a9-852f-ee838fd21c77.png#clientId=ua17b3828-073a-4&from=drop&id=u31b661c7&originHeight=480&originWidth=640&originalType=binary&ratio=2&rotation=0&showTitle=false&size=48144&status=done&style=none&taskId=u464b0a26-5211-4462-bf48-0a061dd56e9&title=)<br />(a) Epsilon-greedy<br />![3-2.png](https://intranetproxy.alipay.com/skylark/lark/0/2023/png/94556578/1688017424500-8560bf4d-4c85-4d71-83b4-c2853cdf6fc4.png#clientId=ueb9e8aed-7604-4&from=paste&height=240&id=u2f0bb22b&originHeight=480&originWidth=640&originalType=binary&ratio=2&rotation=0&showTitle=false&size=50287&status=done&style=none&taskId=u52d60b4e-8f2a-4e2c-86ce-5cfbdc2b091&title=&width=320)<br />(b) DQN<br />![3-2.png](https://intranetproxy.alipay.com/skylark/lark/0/2023/png/94556578/1688015335187-282306e1-e854-4a7c-9869-9d70c0f1e5c8.png#clientId=u960ba388-6074-4&from=paste&height=240&id=udf6aa022&originHeight=480&originWidth=640&originalType=binary&ratio=2&rotation=0&showTitle=false&size=33559&status=done&style=none&taskId=uae935c4d-5b52-4720-ad50-684f5698873&title=&width=320)<br />(c) MWU<br />Fig3.2 Second price simulated results with 2/3/5 bidders with asymmetric valuation [exp3-2]

#### Conclusion
The value of bid(v)/v still converges to around 1.

---

## Reference
[1] [https://web.stanford.edu/~jdlevin/Econ%20286/Auctions.pdf](https://web.stanford.edu/~jdlevin/Econ%20286/Auctions.pdf)<br />[2] Vickrey, William (1961) “Counterspeculation, Auctions and Competitive Sealed Tenders,” Journal of Finance, 16, 8—39.<br />[3] Feng, Zhe & Guruganesh, Guru & Liaw, Christopher & Mehta, Aranyak & Sethi, Abhishek. (2020). Convergence Analysis of No-Regret Bidding Algorithms in Repeated Auctions. <br />[4] Yoav Kolumbus and Noam Nisan. 2022. Auctions between Regret-Minimizing Agents. In Proceedings of the ACM Web Conference 2022 (WWW '22).
