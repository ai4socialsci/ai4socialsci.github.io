## Theoritcal equilibrium in First Price
### Basic setting
We follow the basic setting in our auction simulated environment from [1], which consists of the following components with sealed first price mechanism[2].

- Independent bidders  ![](https://intranetproxy.alipay.com/skylark/lark/__latex/4787ef6775bbb255dcfeea378956293f.svg#card=math&code=i%3D1%2C2..N&id=pECGU)
- Sold items ![](https://intranetproxy.alipay.com/skylark/lark/__latex/78696e84e463662798d8a1dd910ca0e9.svg#card=math&code=j%3D1%2C2..M&id=PfZ0j)
- Independent observed private signal (or valuation)  : ![](https://intranetproxy.alipay.com/skylark/lark/__latex/366696689be40c5e5185beaf02a9308f.svg#card=math&code=S_i%20%5Csim%20F_i%28.%29&id=PbJQ4), with typical realization ![](https://intranetproxy.alipay.com/skylark/lark/__latex/18f310cb9e895ee1182602ba6ca5b9c9.svg#card=math&code=s_i%20%5Cin%5Bs_%7Bmin%7D%2Cs_%7Bmax%7D%5D&id=s4yWM) and assume ![](https://intranetproxy.alipay.com/skylark/lark/__latex/fee5b77b0296d77a9f9b01af5821beb2.svg#card=math&code=F_i&id=ngaSv) is continous 
- Bidder ![](https://intranetproxy.alipay.com/skylark/lark/__latex/2443fbcfeb7e85e1d62b6f5e4f27207e.svg#card=math&code=i&id=S1WKr)'s value is ![](https://intranetproxy.alipay.com/skylark/lark/__latex/559a1ee0f185e637bd82b7bdf87e8fe4.svg#card=math&code=v_i%28s_i%29%20%3D%20s_i&id=iBDFL), which is independent of other bidder's value and information. 
### First auction equilibrium results
According to [1], there is a symmetric equilibrium where every bidder's policy![](https://intranetproxy.alipay.com/skylark/lark/__latex/951beeaa0150701384c7902c424df387.svg#card=math&code=b_i%28s_i%29%20%3D%20b%28s_i%29&id=QWN5T), and![](https://intranetproxy.alipay.com/skylark/lark/__latex/e421edb3025e7a9899d09dbb3a17d4ed.svg#card=math&code=b%28s%29%20%3Ds%20-%5Cfrac%7B%5Cint_%7B%5Cunderline%7Bs%7D%7D%5E%7Bs_i%7DF%5E%7Bn-1%7D%28%5Ctilde%20s%29d%5Ctilde%20s%7D%7BF%5E%7Bn-1%7D%28s%29%7D.&id=Tpvbx)
## Experiment equilibrium validation  
We apply multi-agent methodology to simulate the auction game and compute their best response after their bidding policies converged. We apply no-regret algorithm using **Epsilon-greedy, MWU** and **DQN** to validate the results. The convergence guarantee has been proved in [3,5].

The envirionment and algorithm learning hyperparameter of all the experiment is listed below:

| **Overall Parameter** | **Explanation** |
| --- | --- |
| `args.mechanism = "first_price"` | Auction mechanism: first price sealed auction |
| `args.allocation_mode = "highest"` | Allocation mode: highest bidder win |
| `args.overbid=True` | Whether bidder is allowed to bid more than its value |
| `args.bidding_range` | The range of bidder's bidding value is [0, bidding_range) |
| `args.folder_name` | Plotted figures during the training steps will be saved in './results/{folder_name}/' |
| `args.env_iters ` | Number of iterations in one round (i.e. 'env.step()' in the code) |
| `args.estimate_frequent` | Estimate policy results per {estimate_frequent} iters |
| `args.revenue_averaged_stamp ` | Platform revenue is record and averaged per {revenue_averaged_stamp} iters |
| `args.exploration_epoch` | Number of exploration iters for agent's algorithm |
| `args.algorithm` | Agent adopted algorithm, including [MWU,DQN,Multi_arm_bandit ] |
| `args.step_floor` | ![](https://intranetproxy.alipay.com/skylark/lark/__latex/b6742e9d7880d4f24760074ef7737769.svg#card=math&code=%5Cvarepsilon%20&id=QjmuX)decay parameter |
| `args.buffer_size ` | buffer size in deep rl algorithm |
| `args.batchsize` | batch size for deep rl algorithm |
| `args.lr` | learning rate |


---

### Setting 1. Symmetric i.i.d Uniform distribution  

#### Experiment setting 
##### exp1-1 :  2 bidders in sealed FP auction 
We first validate the results in the **Proposition-1  **with 2 bidders with discrete valuation sampled from Uniform distribution ![](https://intranetproxy.alipay.com/skylark/lark/__latex/19fa58097401a8abe4bb347163258d84.svg#card=math&code=F_i%3DU%5B0%2C10%5D&id=al8tN) where we apply smart agent to simulate the bidder's behaviors and explore his optmial bidding policy. <br />Three algorithm is applied, including epsilon-greedy, DQN and MWU algorithm. <br />For different algorithms, we set:

- `args.env_iters = 2e6, args.exploration_epoch = 2e5` for Epsilon-greedy;
- `args.env_iters = 4e6, args.exploration_epoch = 2e6` for DQN;
- `args.env_iters = 1e5` for MWU.
##### exp1-2 : ablation study : bidders 
We then increase bidder number to validate its effects. We choose agent 0 as an example to display whether his policy will sharply changed when bidder number increase.<br />We test agent 0's learned policy when the bidder number is 2, 3, 4, 5 and 6. All bidders' valuation is sampled from uniform distribution ![](https://intranetproxy.alipay.com/skylark/lark/__latex/19fa58097401a8abe4bb347163258d84.svg#card=math&code=F_i%3DU%5B0%2C10%5D&id=OuQrn).<br />Three algorithm is applied, including epsilon-greedy, DQN and MWU algorithm. <br />For different algorithms, we set:

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
We record the runnable script in the "./Equilibrium_Explore/script/Explore_2_FP/", and we identify the scripts using different algorithms with the filename postfix: "run_fp_exp1.sh", "run_fp_exp1_dqn.sh" or "run_fp_exp1_mwu.sh". And here we use "run_fp_exp2.sh" which uses default epsilon-greedy algorithm as an example:
```python
# EXP1 -- Symmetric i.i.d Uniform distribution
# exp 1-1
python3 Explore_2_FP.py --mode="exp_1" --valuation_range=10 --player_num=2 --exp_id=11 &&
# exp 1-2: increasing player num
python3 Explore_2_FP.py --mode="exp_1" --valuation_range=10 --player_num=3 --exp_id=123 &&
python3 Explore_2_FP.py --mode="exp_1" --valuation_range=10 --player_num=4 --exp_id=124 &&
python3 Explore_2_FP.py --mode="exp_1" --valuation_range=10 --player_num=5 --exp_id=125 &&
python3 Explore_2_FP.py --mode="exp_1" --valuation_range=10 --player_num=6 --exp_id=126 &&
# exp 1-3: different valuation range
python3 Explore_2_FP.py --mode="exp_1" --valuation_range=10 --player_num=4 --exp_id=131 &&
python3 Explore_2_FP.py --mode="exp_1" --valuation_range=20 --player_num=4 --exp_id=132 &&
python3 Explore_2_FP.py --mode="exp_1" --valuation_range=50 --player_num=4 --exp_id=135 &&
python3 Explore_2_FP.py --mode="exp_1" --valuation_range=100 --player_num=4 --exp_id=1310 &&
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
From the theoritcal results, symmetric bidding strategy follows as below <br />![image.png](https://intranetproxy.alipay.com/skylark/lark/0/2023/png/229273/1684487900349-f46df9a7-bdf5-4277-8b52-297142491b2c.png#clientId=ufc3b5be1-5875-4&from=paste&height=94&id=u79a5e304&originHeight=94&originWidth=380&originalType=binary&ratio=1&rotation=0&showTitle=false&size=18571&status=done&style=stroke&taskId=u45baac4f-80df-48f0-be68-eb7d498abcb&title=&width=380)<br />![](https://intranetproxy.alipay.com/skylark/lark/__latex/e321a27789899b0ec40cb08e421b7a4d.svg#card=math&code=%5Ctext%7Bi.e.%20%7Dbid_i%28v_i%29%20%3D%20%5Cfrac%7Bn-1%7D%7Bn%7Dv_i&id=JsklH)
#### Experiment results
##### exp1-1 :  2 bidders in sealed FP auction 
Bidder's bid(v)/v considering different valuation v:<br />![exp_1_valuation_10_player_num_2.png](https://intranetproxy.alipay.com/skylark/lark/0/2023/png/94556578/1685086148347-1ee0ee2f-1028-479b-8078-0af3b64784d5.png#clientId=uc615ab5e-cea1-4&from=drop&id=u021717e2&originHeight=480&originWidth=640&originalType=binary&ratio=2&rotation=0&showTitle=false&size=24783&status=done&style=stroke&taskId=u15c0c32b-5c2f-4249-8b1d-cfce91bfb5c&title=)<br />(a) Epsilon-greedy<br />![exp_1_valuation_10_player_num_2.png](https://intranetproxy.alipay.com/skylark/lark/0/2023/png/94556578/1688019643081-acddf28b-bea0-4774-a29e-ada8a4201070.png#clientId=ud67ad386-16cc-4&from=paste&height=240&id=uc5c8b367&originHeight=480&originWidth=640&originalType=binary&ratio=2&rotation=0&showTitle=false&size=25058&status=done&style=stroke&taskId=u7a7562b1-d8bd-4969-abd0-9ce70698c6c&title=&width=320)<br />(b) DQN<br />![exp_1_valuation_10_player_num_2.png](https://intranetproxy.alipay.com/skylark/lark/0/2023/png/94556578/1687941954452-2c240785-ab76-403f-ab41-6464c32c9dc4.png#clientId=uefbeb71a-fd62-4&from=paste&height=240&id=u8e65ba17&originHeight=480&originWidth=640&originalType=binary&ratio=2&rotation=0&showTitle=false&size=24027&status=done&style=stroke&taskId=u0c8ee68d-a52c-45ee-9f44-41cebf708de&title=&width=320)<br />(c) MWU<br />Fig1.1 First price simulated results with 2 bidders valuation: U[0,10) [exp1-1]

##### exp1-2 : ablation study : bidders 
Agent 0's bidding policy (bid/valuation) when player num= 2/3/4/5/6:<br />![exp1_2.png](https://intranetproxy.alipay.com/skylark/lark/0/2023/png/94556578/1685086774313-fc3b4859-1f96-4e3d-93be-e37e9a0884aa.png#clientId=uc615ab5e-cea1-4&from=drop&id=u98e13c9f&originHeight=480&originWidth=640&originalType=binary&ratio=2&rotation=0&showTitle=false&size=40631&status=done&style=stroke&taskId=u6721b49c-abd2-4946-be46-df18bc285c1&title=)<br />(a) Epsilon-greedy<br />![1-2.png](https://intranetproxy.alipay.com/skylark/lark/0/2023/png/94556578/1688032292839-2ead7ba2-026c-46a9-958d-95f6203dcd08.png#clientId=u9a21045a-046d-4&from=paste&height=240&id=u19be1f11&originHeight=480&originWidth=640&originalType=binary&ratio=2&rotation=0&showTitle=false&size=44681&status=done&style=stroke&taskId=u9614aaaf-7e01-44a7-a2c3-705ae51d434&title=&width=320)<br />(b) DQN<br />![1-2.png](https://intranetproxy.alipay.com/skylark/lark/0/2023/png/94556578/1687946565479-2ede3a20-4059-4578-96f6-bd0eca10b7aa.png#clientId=u009cfa41-f4ab-4&from=paste&height=240&id=u03844730&originHeight=480&originWidth=640&originalType=binary&ratio=2&rotation=0&showTitle=false&size=39303&status=done&style=stroke&taskId=uf917da4f-1cf8-4563-87e2-8045a90318a&title=&width=320)<br />(c) MWU<br />Fig 1.2 First price simulated results with 2~6 bidders valuation: U[0,10) [exp1-2]

##### exp1-3 : ablation study : valuation range  
4 bidders' bidding policy, when the valuation range is [0,10):<br />![exp_1_valuation_10_player_num_4.png](https://intranetproxy.alipay.com/skylark/lark/0/2023/png/94556578/1685086907682-1d631bcf-6269-4b30-b438-7ef687449bb5.png#clientId=uc615ab5e-cea1-4&from=drop&id=u14ffe0b9&originHeight=480&originWidth=640&originalType=binary&ratio=2&rotation=0&showTitle=false&size=30781&status=done&style=stroke&taskId=u5727a6a1-dfbb-4b1f-b716-07e5e8dcd6b&title=)<br />(a) Epsilon-greedy<br />![exp_1_valuation_10_player_num_4.png](https://intranetproxy.alipay.com/skylark/lark/0/2023/png/94556578/1688019679244-33323674-8688-4a3f-b69f-6f5ed5e07a76.png#clientId=ud67ad386-16cc-4&from=paste&height=240&id=u4f1e2c9a&originHeight=480&originWidth=640&originalType=binary&ratio=2&rotation=0&showTitle=false&size=31640&status=done&style=stroke&taskId=u3790ada2-a2de-4b7e-87b7-6f5130e4ae3&title=&width=320)<br />(b) DQN<br />![exp_1_valuation_10_player_num_4.png](https://intranetproxy.alipay.com/skylark/lark/0/2023/png/94556578/1687941933702-9fcd6e5a-a8df-4e31-a67e-4cd526148f65.png#clientId=uefbeb71a-fd62-4&from=paste&height=240&id=ud5be5814&originHeight=480&originWidth=640&originalType=binary&ratio=2&rotation=0&showTitle=false&size=29124&status=done&style=stroke&taskId=ua47566e8-0091-49e6-8b30-4ebaa9d71a0&title=&width=320)<br />(c) MWU<br />Fig 1.3.1 First price simulated results with 4 bidders valuation: U[0,10) [exp1-3]

4 bidders' bidding policy, when the valuation range is [0,20):<br />![exp_1_valuation_20_player_num_4.png](https://intranetproxy.alipay.com/skylark/lark/0/2023/png/94556578/1685086915279-c59ec91b-9ebf-48a6-b1ad-3708c3f5b033.png#clientId=uc615ab5e-cea1-4&from=drop&id=ua28ea4ba&originHeight=480&originWidth=640&originalType=binary&ratio=2&rotation=0&showTitle=false&size=39548&status=done&style=stroke&taskId=u7bebd09d-cb48-4295-9cac-066045432a4&title=)<br />(a) Epsilon-greedy<br />![exp_1_valuation_20_player_num_4.png](https://intranetproxy.alipay.com/skylark/lark/0/2023/png/94556578/1688019672611-3659e8a1-0936-494c-b7c5-e191a0f360b9.png#clientId=ud67ad386-16cc-4&from=paste&height=240&id=ubdeee4ce&originHeight=480&originWidth=640&originalType=binary&ratio=2&rotation=0&showTitle=false&size=35417&status=done&style=stroke&taskId=ub1375b3a-9728-499a-83b3-2c099c75899&title=&width=320)<br />(b) DQN<br />![exp_1_valuation_20_player_num_4.png](https://intranetproxy.alipay.com/skylark/lark/0/2023/png/94556578/1687946272809-2c869737-caf4-4d32-aa04-f5312ae12988.png#clientId=u009cfa41-f4ab-4&from=paste&height=240&id=u524c1209&originHeight=480&originWidth=640&originalType=binary&ratio=2&rotation=0&showTitle=false&size=31179&status=done&style=stroke&taskId=u999f0747-785c-4f4d-bca2-1eb45cc095d&title=&width=320)<br />(c) MWU<br />Fig 1.3.2 First price simulated results with 4 bidders valuation: U[0,20) [exp1-3]

4 bidders' bidding policy, when the valuation range is [0,50):<br />![exp_1_valuation_50_player_num_4.png](https://intranetproxy.alipay.com/skylark/lark/0/2023/png/94556578/1685086922905-f1a1cedc-6406-4f9a-af4e-55e5d4a9565b.png#clientId=uc615ab5e-cea1-4&from=drop&id=u6dd21be6&originHeight=480&originWidth=640&originalType=binary&ratio=2&rotation=0&showTitle=false&size=56666&status=done&style=stroke&taskId=u59b0766c-bf68-468f-8efe-2bc4a467735&title=)<br />(a) Epsilon-greedy<br />![exp_1_valuation_50_player_num_4.png](https://intranetproxy.alipay.com/skylark/lark/0/2023/png/94556578/1688019686508-969d1a81-1baa-47db-867d-b1b059084580.png#clientId=ud67ad386-16cc-4&from=paste&height=240&id=ue6f9e859&originHeight=480&originWidth=640&originalType=binary&ratio=2&rotation=0&showTitle=false&size=41156&status=done&style=stroke&taskId=u439c0fd9-6647-4081-b6e1-1bfb77a021d&title=&width=320)<br />(b) DQN<br />![exp_1_valuation_50_player_num_4.png](https://intranetproxy.alipay.com/skylark/lark/0/2023/png/94556578/1687946280493-5fe760de-0d03-4893-891e-caa6acf2ad1a.png#clientId=u009cfa41-f4ab-4&from=paste&height=240&id=u4ca01149&originHeight=480&originWidth=640&originalType=binary&ratio=2&rotation=0&showTitle=false&size=35587&status=done&style=stroke&taskId=u4937614d-693f-4a88-918c-7685f24fb02&title=&width=320)<br />(c) MWU<br />Fig 1.3.3 First price simulated results with 4 bidders valuation: U[0,50) [exp1-3]

4 bidders' bidding policy, when the valuation range is [0,100):<br />![exp_1_valuation_100_player_num_4.png](https://intranetproxy.alipay.com/skylark/lark/0/2023/png/94556578/1685086928806-a8e2768c-0276-41a2-bc14-34466dc1a2c7.png#clientId=uc615ab5e-cea1-4&from=drop&id=uf7daf636&originHeight=480&originWidth=640&originalType=binary&ratio=2&rotation=0&showTitle=false&size=64196&status=done&style=stroke&taskId=u20679867-b754-4eb8-891b-90611d84a15&title=)<br />(a) Epsilon-greedy<br />![exp_1_valuation_100_player_num_4.png](https://intranetproxy.alipay.com/skylark/lark/0/2023/png/94556578/1688019693448-b1329a49-0e19-44f4-a098-8c8e094dc80c.png#clientId=ud67ad386-16cc-4&from=paste&height=240&id=ua4d7030f&originHeight=480&originWidth=640&originalType=binary&ratio=2&rotation=0&showTitle=false&size=41363&status=done&style=stroke&taskId=u7f164196-2f00-48ca-8acf-de2bbf3820a&title=&width=320)<br />(b) DQN<br />![exp_1_valuation_100_player_num_4.png](https://intranetproxy.alipay.com/skylark/lark/0/2023/png/94556578/1688015617469-bf670d14-1bba-4c9d-b08c-9404bf82d940.png#clientId=uea9b0d59-4d1e-4&from=paste&height=240&id=u6d77aea1&originHeight=480&originWidth=640&originalType=binary&ratio=2&rotation=0&showTitle=false&size=42545&status=done&style=stroke&taskId=u7a6e451b-322b-4c92-bf27-19eee0e7122&title=&width=320)<br />(c) MWU<br />Fig 1.3.4 First price simulated results with 4 bidders valuation: U[0,100) [exp1-3]

#### Conclusion
##### exp1-1 :  2 bidders in sealed FP auction 
As we can see, the bid(v)/v of both player 0 and player 1 converge to around 0.5, which is in line with the theory result.
##### exp1-2 : ablation study : bidders 
In our experiment result, the bid(v)/v of agent 0 increases with the growing of player num. That's because the equilibrium policy's bidding value increases as n (i.e. player num) increases:<br />![](https://intranetproxy.alipay.com/skylark/lark/__latex/f17acb763010795f10f8d76290727c9b.svg#card=math&code=bid%28v%29%20%3D%20%5Cfrac%7Bn-1%7D%7Bn%7Dv%20%3D%20%281-%5Cfrac1n%29v%0A&id=LelEH)
##### exp1-3 : ablation study : valuation range  
As we can see, despite the valuation range's expanding, the bidding policy of 4 players still converges to equilibrium result, i.e. bid(v)/v -> 0.75.

---

### Setting 2. Symmetric i.i.d  Gaussian distribution  
#### Experiment setting 
##### exp2 :  4 bidders in sealed FP auction 
Here we consider different valuation distribution's impact on bidder's policy. We validate the results** **with 4 bidders with discrete valuation sampled from Gaussian distribution ![](https://intranetproxy.alipay.com/skylark/lark/__latex/cc79e457fdbd3b1baa704204b42e5745.svg#card=math&code=F_i%3D%5Cmathcal%20N%2850%2C16%29&id=tlbNc) where we apply smart agent to simulate the bidder's behaviors and explore his optmial bidding policy. <br />Three algorithm is applied, including epsilon-greedy, DQN and MWU algorithm. <br />For different algorithms, we set:

- `args.env_iters = 2e6, args.exploration_epoch = 2e5` for Epsilon-greedy;
- `args.env_iters = 4e6, args.exploration_epoch = 2e6` for DQN;
- `args.env_iters = 1e6` for MWU.
##### hyper parameters

- in the DQN  algorithm, we set `args.batchsize = 128 , args.lr = 5e-4`
- in the MWU algorithm, we set `args.lr = 0.05 `
#### runnable script
We record the runnable script in the "./Equilibrium_Explore/script/Explore_2_FP/run_fp_exp2.sh",
```python
# EXP2 -- Symmetric i.i.d  Gaussian distribution
python3 Explore_2_FP.py --mode="exp_2" --valuation_range=100 --player_num=4 --exp_id=2 &&
```
We also illustrate the core hyperparameters as below, 

| **Core Parameter** | **Explanation** |
| --- | --- |
| `mode` | Different experiment mode |
| `valuation_range` | Valuation_range is the upper_bound of bidder's value. <br />In exp2, bidder's valuation is sampled from gaussian distribution where mean=valuation_range/2, and sigma= valuation_range/6, so that the support approximates [0, valuation_range). |
| `player_num` | Number of bidders |
| `exp_id` | Result saving id |
| `algorithm` | DQN or MWU or default algorithm(epsilon-greedy) |
| `gpu` | GPU id for training deep rl algorithm |

#### Theoritical results
We can calculate the theoretical result with![](https://intranetproxy.alipay.com/skylark/lark/__latex/fb6d28cf1b904f2205c5c3748ae4f995.svg#card=math&code=b%28s%29%20%3Ds%20-%5Cfrac%7B%5Cint_%7B%5Cunderline%7Bs%7D%7D%5E%7Bs_i%7DF%5E%7Bn-1%7D%28%5Ctilde%20s%29d%5Ctilde%20s%7D%7BF%5E%7Bn-1%7D%28s%29%7D%2C&id=KJ3yY)where![](https://intranetproxy.alipay.com/skylark/lark/__latex/f65a9d5b271ae9903ce1f46ba85ed70f.svg#card=math&code=F_i%3D%5Cmathcal%20N%2850%2C16%29.&id=f1l1k)<br />While the closed-form solution is too complicated, we can calculate the the bidding function with some symbolic computation software, and plot the result as follow:<br />![gaussian_numerical.png](https://intranetproxy.alipay.com/skylark/lark/0/2023/png/94556578/1685071989687-0fa8b183-90d3-4aa2-bf80-517054321062.png#clientId=ub1e451d2-65e3-4&from=drop&id=u1b4b39f0&originHeight=480&originWidth=640&originalType=binary&ratio=2&rotation=0&showTitle=false&size=22233&status=done&style=stroke&taskId=u8b103051-8587-4d6c-98c3-812ee5eb0f1&title=)<br />Fig2.1 First price numerical solution of 4 bidders Gaussian distribution's valuation
#### Experiment results
4 bidders' bid(v)/v considering different valuation v:<br />![exp_2_valuation_100_player_num_4.png](https://intranetproxy.alipay.com/skylark/lark/0/2023/png/94556578/1685086941830-2efbc60f-74d9-4d22-8b9c-77ab62afa5f0.png#clientId=uc615ab5e-cea1-4&from=drop&id=u21d1d8f0&originHeight=480&originWidth=640&originalType=binary&ratio=2&rotation=0&showTitle=false&size=68400&status=done&style=stroke&taskId=u4c88f08f-11d7-4bc5-9799-00201b12852&title=)<br />(a) Epsilon-greedy<br />![exp_2_valuation_100_player_num_4.png](https://intranetproxy.alipay.com/skylark/lark/0/2023/png/94556578/1688019707078-cff5308f-6e6a-4b68-bc52-1a73cae2449f.png#clientId=ud67ad386-16cc-4&from=paste&height=240&id=ueb9aa9be&originHeight=480&originWidth=640&originalType=binary&ratio=2&rotation=0&showTitle=false&size=40539&status=done&style=stroke&taskId=ua9cda958-e945-4518-b918-154336f18e5&title=&width=320)<br />(b) DQN<br />![exp_2_valuation_100_player_num_4.png](https://intranetproxy.alipay.com/skylark/lark/0/2023/png/94556578/1688015630699-4bb360dd-674b-43b0-8dbc-9975be3f4e47.png#clientId=uea9b0d59-4d1e-4&from=paste&height=240&id=u3e45ae21&originHeight=480&originWidth=640&originalType=binary&ratio=2&rotation=0&showTitle=false&size=41036&status=done&style=stroke&taskId=u054e21e9-759b-41fb-a008-7bf312607a0&title=&width=320)<br />(c) MWU<br />Fig2.2 First price simulated results with 4 bidders Gaussian distribution's valuation [exp2]
#### Conclusion
As we can see in Fig2.1 & Fig2.2, the trend of bid(v)/v value is consistent with the theoretical prediction.

---

### Setting 3. Asymmetric i.i.d Uniform distribution  
#### Experiment setting 
##### exp3-1 :  2 bidders in sealed FP auction 
We explore the results in the **asymmetirc situation **with 2 bidders[4] with discrete valuation sampled from Uniform distribution, where we apply smart agent to simulate the bidder's behaviors and explore his optmial bidding policy. <br />![](https://intranetproxy.alipay.com/skylark/lark/__latex/4181148f4b2ff23dd1b79867c2eef7a3.svg#card=math&code=F_1%3DU%5B0%2Cw_1%5D%2C%20F_2%3D%5B0%2Cw_2%5D&id=qvQam)<br />and we choose ![](https://intranetproxy.alipay.com/skylark/lark/__latex/2b966c5c5bd75c2efd46011b134d163e.svg#card=math&code=w_1%20%3D%2010&id=L69XO) ,![](https://intranetproxy.alipay.com/skylark/lark/__latex/c46acf05552235bc21051b473d56c1f0.svg#card=math&code=w_2%20%3D%2020&id=dwAM5)and ![](https://intranetproxy.alipay.com/skylark/lark/__latex/2372631aad98fbab1def1d753ceb1cd1.svg#card=math&code=w_1%20%3D%2020&id=qGmcx) ,![](https://intranetproxy.alipay.com/skylark/lark/__latex/b0e1aca11802f0de5b61ba46a2223936.svg#card=math&code=w_2%20%3D%2040&id=oFk9R).<br />Three algorithm is applied, including epsilon-greedy, DQN and MWU algorithm. <br />For different algorithms, we set:

- `args.env_iters = 2e6, args.exploration_epoch = 2e5` for Epsilon-greedy;
- `args.env_iters = 4e6, args.exploration_epoch = 2e6` for DQN;
- `args.env_iters = 2e5/4e5` for MWU with![](https://intranetproxy.alipay.com/skylark/lark/__latex/d2aca63c4ea01739097291ebf7c5b05b.svg#card=math&code=w_2%20%3D%2020%2F40&id=BL5ss)respectively.
##### exp3-2 :  4 bidders in sealed FP auction 
We explore the results in the **asymmetirc situation **with 4 bidders[4] with discrete valuation sampled from Uniform distribution, where we apply smart agent to simulate the bidder's behaviors and explore his optmial bidding policy. <br />![](https://intranetproxy.alipay.com/skylark/lark/__latex/79f468964252c9909e6357a3f0f1ef51.svg#card=math&code=F_1%3DU%5B0%2C5%5D%2C%20F_2%3D%5B5%2C10%5D%2CF_3%3DU%5B0%2C10%5D%2C%20F_4%3D%5B2%2C8%5D%2C&id=fs4Sy)<br />Three algorithm is applied, including epsilon-greedy, DQN and MWU algorithm. <br />For different algorithms, we set:

- `args.env_iters = 2e6, args.exploration_epoch = 2e5` for Epsilon-greedy;
- `args.env_iters = 4e6, args.exploration_epoch = 2e6` for DQN;
- `args.env_iters = 1e5` for MWU.
##### hyper parameters

- in the DQN  algorithm, we set `args.batchsize = 128 , args.lr = 5e-4`
- in the MWU algorithm, we set `args.lr = 0.05 `
#### runnable script
We record the runnable script in the "./Equilibrium_Explore/script/Explore_2_FP/run_fp_exp2.sh",
```python
# EXP3 -- Asymmetric i.i.d Uniform distribution
# exp 3-1
# player 0's valuation : [0,10], player 1's valuation: [0,20]
python3 Explore_2_FP.py --mode="exp_3_1" --valuation_range=21 --player_num=2 --exp_id=311 &&
# player 0's valuation : [0,20], player 1's valuation: [0,40]
python3 Explore_2_FP.py --mode="exp_3_1" --valuation_range=41 --player_num=2 --exp_id=312 &&
# exp 3-2
# 4 player's valuation: [0,5], [5,10], [0,10], [2,8]
python3 Explore_2_FP.py --mode="exp_3_2" --valuation_range=11 --player_num=4 --exp_id=32
```
We also illustrate the core hyperparameters as below, 

| **Core Parameter** | **Explanation** |
| --- | --- |
| `mode` | Different experiment mode |
| `valuation_range` | Valuation_range is the upper_bound of bidder's value.  <br />In exp3_1, bidder 0's value is sampled from [0, (valuation_range-1)/2], and bidder 1's value is sampled from [0, valuation_range-1];<br />While in exp3_2, bidders' value is sampled from [0,5], [5,10], [0,10], [2,8] respectively. |
| `player_num` | Number of bidders |
| `exp_id` | Result saving id |
| `algorithm` | DQN or MWU or default algorithm(epsilon-greedy) |
| `gpu` | GPU id for training deep rl algorithm |

#### Theoritical results
##### exp 3-1:
![](https://intranetproxy.alipay.com/skylark/lark/__latex/920442ef286d3740a133d35b41ff77c6.svg#card=math&code=%F0%9D%91%98_%F0%9D%91%96%3D1%2F%28%F0%9D%91%A4_%F0%9D%91%96%5E2%20%29%E2%88%921%2F%28%F0%9D%91%A4_%F0%9D%91%97%5E2%20%29%2C%20%F0%9D%91%8F_%F0%9D%91%96%20%28%F0%9D%91%A5%29%3D1%2F%28%F0%9D%91%98_%F0%9D%91%96%20%F0%9D%91%A5%29%281%E2%88%92%E2%88%9A%281%E2%88%92%F0%9D%91%98_%F0%9D%91%96%20%F0%9D%91%A5%5E2%29%29&id=kTMlZ)<br />And we can plot the theoretical results of our experiment setting as follows:<br />![3-1-1.png](https://intranetproxy.alipay.com/skylark/lark/0/2023/png/94556578/1685346800130-4fe23183-e1fc-4334-af1b-935b24d8a5d0.png#clientId=ubdaf2177-1aa0-4&from=drop&id=ueb92184a&originHeight=480&originWidth=640&originalType=binary&ratio=2&rotation=0&showTitle=false&size=23304&status=done&style=stroke&taskId=ucb60b161-3215-460d-b0fa-7060ecd94d0&title=)bid/value when![](https://intranetproxy.alipay.com/skylark/lark/__latex/2b966c5c5bd75c2efd46011b134d163e.svg#card=math&code=w_1%20%3D%2010&id=NlmMB) ,![](https://intranetproxy.alipay.com/skylark/lark/__latex/c46acf05552235bc21051b473d56c1f0.svg#card=math&code=w_2%20%3D%2020&id=s8HXZ)<br />![3-1-2.png](https://intranetproxy.alipay.com/skylark/lark/0/2023/png/94556578/1685346803818-775f799d-abb7-4235-8c71-c00ba1c4cd5b.png#clientId=ubdaf2177-1aa0-4&from=drop&id=uff120869&originHeight=480&originWidth=640&originalType=binary&ratio=2&rotation=0&showTitle=false&size=22364&status=done&style=stroke&taskId=uad4421ee-c662-402e-9f66-dca1e63ae4c&title=)bid/value when![](https://intranetproxy.alipay.com/skylark/lark/__latex/2372631aad98fbab1def1d753ceb1cd1.svg#card=math&code=w_1%20%3D%2020&id=TDSsl) ,![](https://intranetproxy.alipay.com/skylark/lark/__latex/b0e1aca11802f0de5b61ba46a2223936.svg#card=math&code=w_2%20%3D%2040&id=G4CCm)
#### Experiment results
##### exp3-1 :  2 bidders in sealed FP auction 
Learned bidding policy when ![](https://intranetproxy.alipay.com/skylark/lark/__latex/2b966c5c5bd75c2efd46011b134d163e.svg#card=math&code=w_1%20%3D%2010&id=tDX3T) ,![](https://intranetproxy.alipay.com/skylark/lark/__latex/c46acf05552235bc21051b473d56c1f0.svg#card=math&code=w_2%20%3D%2020&id=DeBl9):<br />![exp_3_1_valuation_21_player_num_2.png](https://intranetproxy.alipay.com/skylark/lark/0/2023/png/94556578/1685086864197-1f122f13-b18d-4728-a54e-17d93b65e818.png#clientId=uc615ab5e-cea1-4&from=drop&id=u0e59accf&originHeight=480&originWidth=640&originalType=binary&ratio=2&rotation=0&showTitle=false&size=30885&status=done&style=stroke&taskId=u344bc59b-9878-4a60-ba64-a15fb23de10&title=)<br />(a) Epsilon-greedy<br />![exp_3_1_valuation_21_player_num_2.png](https://intranetproxy.alipay.com/skylark/lark/0/2023/png/94556578/1688019721208-67cc49b0-2a67-47c8-974a-80ac6b78c8d0.png#clientId=ud67ad386-16cc-4&from=paste&height=240&id=u42eab102&originHeight=480&originWidth=640&originalType=binary&ratio=2&rotation=0&showTitle=false&size=28821&status=done&style=stroke&taskId=u94d1f76a-3930-4a98-90c6-62d8fe07648&title=&width=320)<br />(b) DQN<br />![exp_3_1_valuation_21_player_num_2.png](https://intranetproxy.alipay.com/skylark/lark/0/2023/png/94556578/1688015675071-81a94de5-e3f8-485d-8ff1-f99b2ea73211.png#clientId=uea9b0d59-4d1e-4&from=paste&height=240&id=u23e7b4c7&originHeight=480&originWidth=640&originalType=binary&ratio=2&rotation=0&showTitle=false&size=28764&status=done&style=stroke&taskId=u4844d44f-321a-470f-8f4a-3003b6c4da4&title=&width=320)<br />(c) MWU<br />Fig3.1 Asymmetric first price results when w1=10, w2=20 [exp3-1]<br />Learned bidding policy when ![](https://intranetproxy.alipay.com/skylark/lark/__latex/2372631aad98fbab1def1d753ceb1cd1.svg#card=math&code=w_1%20%3D%2020&id=TIL0b) ,![](https://intranetproxy.alipay.com/skylark/lark/__latex/b0e1aca11802f0de5b61ba46a2223936.svg#card=math&code=w_2%20%3D%2040&id=EY1RS):<br />![exp_3_1_valuation_41_player_num_2.png](https://intranetproxy.alipay.com/skylark/lark/0/2023/png/94556578/1685086870681-333bffab-be70-4bea-9424-4dcf729eb26b.png#clientId=uc615ab5e-cea1-4&from=drop&id=uaddf8c14&originHeight=480&originWidth=640&originalType=binary&ratio=2&rotation=0&showTitle=false&size=33177&status=done&style=stroke&taskId=u9ea7f46e-3c6f-40a8-93f6-f4636cdbe15&title=)<br />(a) Epsilon-greedy<br />![exp_3_1_valuation_41_player_num_2.png](https://intranetproxy.alipay.com/skylark/lark/0/2023/png/94556578/1688019727996-864c3d6c-3d22-4850-bafd-c81041713528.png#clientId=ud67ad386-16cc-4&from=paste&height=240&id=ua4d05eb7&originHeight=480&originWidth=640&originalType=binary&ratio=2&rotation=0&showTitle=false&size=33471&status=done&style=stroke&taskId=ua0faab3e-35bd-42e7-ab1e-876348b88f4&title=&width=320)<br />(b) DQN<br />![exp_3_1_valuation_41_player_num_2.png](https://intranetproxy.alipay.com/skylark/lark/0/2023/png/94556578/1688015687926-0047a97d-1ef3-4fe4-b2ec-05e1b4b49246.png#clientId=uea9b0d59-4d1e-4&from=paste&height=240&id=u71957ab6&originHeight=480&originWidth=640&originalType=binary&ratio=2&rotation=0&showTitle=false&size=28890&status=done&style=stroke&taskId=u15556d33-4239-4a7c-bb03-0188b2a9a72&title=&width=320)<br />(c) MWU<br />Fig3.1 Asymmetric 2 players first price results when w1=20, w2=40 [exp3-1]
##### exp3-2 :  4 bidders in sealed FP auction 
Learned bidding policy when ![](https://intranetproxy.alipay.com/skylark/lark/__latex/3e2f8fb5915b345196f484029c4dac87.svg#card=math&code=F_1%3DU%5B0%2C5%5D%2C%20F_2%3D%5B5%2C10%5D%2CF_3%3DU%5B0%2C10%5D%2C%20F_4%3D%5B2%2C8%5D%3A&id=vVCB4)<br />![exp_3_2_valuation_11_player_num_4.png](https://intranetproxy.alipay.com/skylark/lark/0/2023/png/94556578/1685086883518-257d5366-50ca-4c74-a051-a902a2f39311.png#clientId=uc615ab5e-cea1-4&from=drop&id=u63c1d6e6&originHeight=480&originWidth=640&originalType=binary&ratio=2&rotation=0&showTitle=false&size=32783&status=done&style=stroke&taskId=u7f423b64-01bf-418a-b8dd-71b72d94319&title=)<br />(a) Epsilon-greedy<br />![exp_3_2_valuation_11_player_num_4.png](https://intranetproxy.alipay.com/skylark/lark/0/2023/png/94556578/1688019735125-f5ef7cdc-2de7-4c7b-99b4-1caefffb97c3.png#clientId=ud67ad386-16cc-4&from=paste&height=240&id=udfde8865&originHeight=480&originWidth=640&originalType=binary&ratio=2&rotation=0&showTitle=false&size=29989&status=done&style=stroke&taskId=u5ab9dc89-a9bb-40d5-a1ee-fecc782c669&title=&width=320)<br />(b) DQN<br />![exp_3_2_valuation_11_player_num_4.png](https://intranetproxy.alipay.com/skylark/lark/0/2023/png/94556578/1688015707413-4be6907b-c913-4a5d-a792-8579a81db3fd.png#clientId=uea9b0d59-4d1e-4&from=paste&height=240&id=u2a217d88&originHeight=480&originWidth=640&originalType=binary&ratio=2&rotation=0&showTitle=false&size=29799&status=done&style=stroke&taskId=uf93a5392-a232-4aed-875b-ac3ffe0f2eb&title=&width=320)<br />(c) MWU<br />Fig3.2 Asymmetric 4 players first price results with valuation=F1/F2/F3/F4 [exp3-2]
#### Conclusion
##### exp3-1 :  2 bidders in sealed FP auction 
According to the solution: ![](https://intranetproxy.alipay.com/skylark/lark/__latex/920442ef286d3740a133d35b41ff77c6.svg#card=math&code=%F0%9D%91%98_%F0%9D%91%96%3D1%2F%28%F0%9D%91%A4_%F0%9D%91%96%5E2%20%29%E2%88%921%2F%28%F0%9D%91%A4_%F0%9D%91%97%5E2%20%29%2C%20%F0%9D%91%8F_%F0%9D%91%96%20%28%F0%9D%91%A5%29%3D1%2F%28%F0%9D%91%98_%F0%9D%91%96%20%F0%9D%91%A5%29%281%E2%88%92%E2%88%9A%281%E2%88%92%F0%9D%91%98_%F0%9D%91%96%20%F0%9D%91%A5%5E2%29%29&id=nt3mQ), the value of ![](https://intranetproxy.alipay.com/skylark/lark/__latex/8701615dc89b15ae2bb2526fa409d183.svg#card=math&code=bid_i%28x_i%29%2Fx_i&id=sSRwL) is around 0.5 for both bidder in general situation, while ![](https://intranetproxy.alipay.com/skylark/lark/__latex/e1b7855e2e18f4e9657c3faa8e1a0771.svg#card=math&code=bid_1%28x_1%29%2Fx_1&id=RBJuv)tends to be higher than 0.5 and![](https://intranetproxy.alipay.com/skylark/lark/__latex/0eb07d3c35da437f2685bad28f96f37c.svg#card=math&code=bid_2%28x_2%29%2Fx_2&id=DEJjA)tends to be lower than 0.5 as the valuation increases, which is also the case in our experiment results.
##### exp3-2 :  4 bidders in sealed FP auction 
The 4 bidders show different bidding policies under asymmetric valuation.

---

## Reference
[1] [https://web.stanford.edu/~jdlevin/Econ%20286/Auctions.pdf](https://web.stanford.edu/~jdlevin/Econ%20286/Auctions.pdf)<br />[2] [https://www.cs.ubc.ca/~cs532l/gt2/slides/11-4.pdf](https://www.cs.ubc.ca/~cs532l/gt2/slides/11-4.pdf)<br />[3] Feng, Zhe & Guruganesh, Guru & Liaw, Christopher & Mehta, Aranyak & Sethi, Abhishek. (2020). Convergence Analysis of No-Regret Bidding Algorithms in Repeated Auctions. <br />[4] Asymmetric first-price auctions with uniform distributions: analytic solutions to the general case<br />[5] Yoav Kolumbus and Noam Nisan. 2022. Auctions between Regret-Minimizing Agents. In Proceedings of the ACM Web Conference 2022 (WWW '22).
