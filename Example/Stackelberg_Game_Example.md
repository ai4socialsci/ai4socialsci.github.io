## Introduction -- Stackelberg game example setting
**Background: **There are 3 suppliers compete with each other, selling identical products. At each round, every supplier receives its cost and bids a price. Who bids the **lowest price** will win the opportunity to sell its product **at the price it bids**.<br />At first, everyone seeks to **maximize its own revenue, i.e. (price - cost) * allocation.** Then **player 0** decides to reduce its price to seize the market, by **maximizing its income, i.e. price * allocation.**<br />**Experimental setting: **In a auction environment, 3 bidders' **costs** are denoted as **c****i**** ~ Fi **respectively. At each round, every bidder observes its cost **c****i**** **bids a price according to its **policy: b****i****=π(c****i****)**, then receive a reward (revenue or income), depending on whether they win the auction and how much they have bid. <br />The game is divided into 3 steps as follows:

1. **Every bidder maximize its revenue**, which will result in a **symmetric equilequilibrium**.
2. After that, **Player 0** decides to **reduce its price** according to a discount policy: **discout=π****d****(c****i****)**. We formulate this problem as a **simplified Stackelberg game**:
   1. Firstly, Player 0 update its discount policy, while other players fix their bid policy.
   2. Then, Player 0 fixs its discount policy, while other players update their bid policies.
   3. Additionally, we consider **information's impact** on this game: if Player 0 reveals a signal before it makes a discount decision, which will be observed by other players, how will the new equilibrium be like?<br />So here we let player 0 to truthfully reveal its discount decision, and other players bid with **b****i****=π(c****i****, discount)**.
3. Finally, We put all agents in a **full Stackelberg game**, where leader is player 0 who make discount, reveal signal and maximize its income, while followers are other players who receives signal and bid to maximize their revenue. The follwing 2 steps will loop to calculate a Stackelberg game equilibrium:
   1. Leader update its policy(discount or signal), follwers fix policy
   2. Leader fixs its policy, followers update.
## Implement with AI4SC platform
### Run with command
In order to run the example game above, please run:
> cd Stackelberg_Game
> python3 stackelberg_game_example.py

The results will be plotted and saved in Stackelberg_Game/results/stackelberg/
### Step1 -- **Every player bids to maximize its revenue**
We need to set environment according to the setting (everyone updates policy, no discount, no information) firstly, then run the env and get results:
```python
# round 0
# everyone update bid policy, no discount, no information
dynamic_env.turnon_follower_agent_update()
dynamic_env.turnon_leading_agent_update()
dynamic_env.disable_discount()
dynamic_env.disable_information()

platform_reward_r0 = dynamic_env.step()  # run the mini env and update policy

# export agent algo
trained_agents = dynamic_env.export_all_agent()
```
**Results (follower update, no discount, no info)**<br />bid policy![agt_3_overbid_customed_epoch_800000.png](https://intranetproxy.alipay.com/skylark/lark/0/2023/png/94556578/1683706623177-ba6a52a9-bf27-4c6e-bc74-bffb90ad9ba5.png#clientId=uc37b0d9d-f2d0-4&from=drop&id=u948d3710&originHeight=480&originWidth=640&originalType=binary&ratio=2&rotation=0&showTitle=false&size=27324&status=done&style=none&taskId=uee6160b3-d3be-4e60-a88b-20a5c80bccf&title=)<br />leader revenue![leader_revenue_agt_3_overbid_customed_epoch_800000avg_total_reward_on_10000.png](https://intranetproxy.alipay.com/skylark/lark/0/2023/png/94556578/1683706643234-84760aa0-fc9f-4bd3-bebd-f6d61f137e87.png#clientId=uc37b0d9d-f2d0-4&from=drop&id=u10362fd4&originHeight=480&originWidth=640&originalType=binary&ratio=2&rotation=0&showTitle=false&size=51082&status=done&style=none&taskId=u74b61806-f8fd-43ba-92f8-5a31caee39d&title=)<br />follower revenue![follower_agt_3_overbid_customed_epoch_800000avg_total_reward_on_10000.png](https://intranetproxy.alipay.com/skylark/lark/0/2023/png/94556578/1683706640875-89c10bd4-c788-4725-89c2-62578568a25e.png#clientId=uc37b0d9d-f2d0-4&from=drop&id=ua0d5ba3b&originHeight=480&originWidth=640&originalType=binary&ratio=2&rotation=0&showTitle=false&size=60122&status=done&style=none&taskId=u83701428-94b9-415d-aa3c-c21d7b92b62&title=)
### Step2 -- Player 0 discount
#### **Step 2.1: Player 0 update discount policy, others fix policy**
Set environment with 2.1 setting (leader/follwer bid policy no update, enable discount and update leader discount policy, no information, leader maximize income), then copy trained agents and set them to environment. (Note: here we need to augment leader agent with discount/info policy with "set_leader_algorithm" function). After that, just run env and save results:
```python
# round 1  decrease + others not update

# leader/follower bid policy no update, leader discount policy update
# enable discount, no information
dynamic_env.re_init()
dynamic_env.turnoff_leading_agent_update()
dynamic_env.turnoff_follower_agent_update()
dynamic_env.enable_discount()
dynamic_env.turnon_leader_discount_update()
dynamic_env.disable_information()
dynamic_env.turnoff_leader_info_update()
# set reward mode for leader
dynamic_env.adjust_reward_mode(reward_mode='income')

# set agents 
# set leader
leader_agt = deepcopy(trained_agents[leading_agent_name])
leader_agt = set_leader_algorithm(leader_agt, args)
dynamic_env.set_rl_agent(rl_agent=leader_agt, agent_name=leading_agent_name)
# set folllowers
for agent_name, agt in trained_agents.items():
    if agent_name != leading_agent_name:
        dynamic_env.set_rl_agent(rl_agent=deepcopy(agt), agent_name=agent_name)

# run env
platform_reward_r1 = dynamic_env.step()  # run the mini env and update policy

# export agent algo
trained_agent_list_r1 = dynamic_env.export_all_agent()
```
**Results (no follower update, discount, no info)**<br />bid policy (leader reduction)![agt_3_overbid_customed_epoch_800000.png](https://intranetproxy.alipay.com/skylark/lark/0/2023/png/94556578/1683706781120-043a47c5-f2c0-4dde-9c5a-02f624d86ade.png#clientId=uc37b0d9d-f2d0-4&from=drop&id=u1f994833&originHeight=480&originWidth=640&originalType=binary&ratio=2&rotation=0&showTitle=false&size=31128&status=done&style=none&taskId=u13ef15b8-6a83-4a18-b21e-d017a346c53&title=)<br />leader income![leader_income_agt_3_overbid_customed_epoch_800000avg_total_reward_on_10000.png](https://intranetproxy.alipay.com/skylark/lark/0/2023/png/94556578/1683706790314-5ccb5f08-e1e1-4a6b-a4cc-dc5518b2b663.png#clientId=uc37b0d9d-f2d0-4&from=drop&id=u5577d46d&originHeight=480&originWidth=640&originalType=binary&ratio=2&rotation=0&showTitle=false&size=54556&status=done&style=none&taskId=uc63970e8-a179-43a7-a4a2-8cfe7dbf5c1&title=)<br />follower revenue![follower_agt_3_overbid_customed_epoch_800000avg_total_reward_on_10000.png](https://intranetproxy.alipay.com/skylark/lark/0/2023/png/94556578/1683706792528-04c3a118-72fd-48ed-9ce3-956d4ad6380c.png#clientId=uc37b0d9d-f2d0-4&from=drop&id=u0e026af5&originHeight=480&originWidth=640&originalType=binary&ratio=2&rotation=0&showTitle=false&size=80683&status=done&style=none&taskId=u2810c44c-a5f8-4c63-945a-80fb98fcb50&title=)
#### Step2.2: Player 0 fix policy, others update bid policy
Set environment with 2.2 setting (leader bid/discount policy no update,  follower update bid policy, enable discount, no information, leader maximize income), then copy trained agents and set them to environment, and run env, save results:
```python
# round 2 --- follower update

# leader bid policy no update, leader discount policy no update
# follower bid policy update
# enable discount, no information
dynamic_env.re_init()
dynamic_env.turnoff_leading_agent_update()
dynamic_env.turnon_follower_agent_update()
dynamic_env.enable_discount()
dynamic_env.turnoff_leader_discount_update()
dynamic_env.disable_information()
dynamic_env.turnoff_leader_info_update()
# set reward mode for leader
dynamic_env.adjust_reward_mode(reward_mode='income')

# set agents
# set leader
leader_agt = deepcopy(trained_agent_list_r1[leading_agent_name])
dynamic_env.set_rl_agent(rl_agent=leader_agt, agent_name=leading_agent_name)
# set folllowers
for agent_name, agt in trained_agents.items():
    if agent_name != leading_agent_name:
        new_agent = deepcopy(agt)
        # reset algorithm for better training (optional)
        new_agent.reset_algo()
        dynamic_env.set_rl_agent(rl_agent=new_agent, agent_name=agent_name)

# run env
platform_reward_r2 = dynamic_env.step()  # run the mini env and update policy

# export agent algo
trained_agent_list_r2 = dynamic_env.export_all_agent()
```
**Results (follwer update, discount, no info)**<br />bid policy (all reduction)![agt_3_overbid_customed_epoch_800000.png](https://intranetproxy.alipay.com/skylark/lark/0/2023/png/94556578/1683706858138-b60866a6-0ddb-4293-8eac-25057e5136c2.png#clientId=uc37b0d9d-f2d0-4&from=drop&id=u4bb6755c&originHeight=480&originWidth=640&originalType=binary&ratio=2&rotation=0&showTitle=false&size=29099&status=done&style=none&taskId=u6711dee2-951c-4075-9695-ace905b2758&title=)<br />leader income![leader_income_agt_3_overbid_customed_epoch_800000avg_total_reward_on_10000.png](https://intranetproxy.alipay.com/skylark/lark/0/2023/png/94556578/1683706876545-73b4567c-184f-435a-b356-618e27b886ce.png#clientId=uc37b0d9d-f2d0-4&from=drop&id=u7378cf17&originHeight=480&originWidth=640&originalType=binary&ratio=2&rotation=0&showTitle=false&size=49927&status=done&style=none&taskId=u900a9470-9287-450c-9a21-5e1e4c4287c&title=)<br />follower revenue![follower_agt_3_overbid_customed_epoch_800000avg_total_reward_on_10000.png](https://intranetproxy.alipay.com/skylark/lark/0/2023/png/94556578/1683706879059-4a0f9604-7ee0-441d-98ec-454a389085d1.png#clientId=uc37b0d9d-f2d0-4&from=drop&id=u663f8864&originHeight=480&originWidth=640&originalType=binary&ratio=2&rotation=0&showTitle=false&size=58046&status=done&style=none&taskId=u3391f3db-8001-4e94-8b2d-820b4b14857&title=)
#### Step2.3: Player 0 fix policy and truthfully reveal, others update bid policy
Set environment with 2.3 setting (leader bid/discount/info policy no update (info policy is truthful), follower update bid policy, enable discount and information, leader maximize income), then copy trained agents and set them to environment, and run env, save results:
```python
# round 3 --- information reveal

# leader bid/discount policy no update, leader info policy no update(truthful)
# follower bid policy update
# enable discount, enable info
dynamic_env.re_init()
dynamic_env.turnoff_leading_agent_update()
dynamic_env.turnon_follower_agent_update()
dynamic_env.enable_discount()
dynamic_env.turnoff_leader_discount_update()
dynamic_env.enable_information()
dynamic_env.turnoff_leader_info_update()
dynamic_env.init_truthful_info()  # initialize information (set truthfully reveal)
# set reward mode for leader
dynamic_env.adjust_reward_mode(reward_mode='income')

# set agents
# set leader
leader_agt = deepcopy(trained_agent_list_r1[leading_agent_name])
dynamic_env.set_rl_agent(rl_agent=leader_agt, agent_name=leading_agent_name)
# set folllowers
for agent_name, agt in trained_agents.items():
    if agent_name != leading_agent_name:
        new_agent = deepcopy(agt)
        # reset algorithm for better training (optional)
        new_agent.reset_algo()
        dynamic_env.set_rl_agent(rl_agent=new_agent, agent_name=agent_name)

# run env
platform_reward_r3 = dynamic_env.step()  # run the mini env and update policy each
# export agent algo
trained_agent_list_r3 = dynamic_env.export_all_agent()
```
**Results (follwer update, discount, info)**<br />leader bid policy![player_0_policy_epoch_2400000.png](https://intranetproxy.alipay.com/skylark/lark/0/2023/png/94556578/1683706959547-01f375fc-a0e5-4db3-9918-eec67e8fafd3.png#clientId=uc37b0d9d-f2d0-4&from=drop&id=u7733b188&originHeight=480&originWidth=640&originalType=binary&ratio=2&rotation=0&showTitle=false&size=17317&status=done&style=none&taskId=u9b2de92f-6dca-41d1-8d28-38453b339a1&title=)<br />P1 bid policy![player_1_policy_epoch_2400000.png](https://intranetproxy.alipay.com/skylark/lark/0/2023/png/94556578/1683708747934-6de7171e-e026-4791-a290-7b7f02a513f3.png#clientId=u5e2ac834-266f-4&from=drop&id=u06394b03&originHeight=480&originWidth=640&originalType=binary&ratio=2&rotation=0&showTitle=false&size=43068&status=done&style=none&taskId=u490ddfb2-d44d-41e0-b94f-1af6c51c819&title=)<br />P2 bid policy![player_2_policy_epoch_2400000.png](https://intranetproxy.alipay.com/skylark/lark/0/2023/png/94556578/1683708749914-ebddca00-1560-428a-b345-8f996181b2b4.png#clientId=u5e2ac834-266f-4&from=drop&id=u0eadac12&originHeight=480&originWidth=640&originalType=binary&ratio=2&rotation=0&showTitle=false&size=36717&status=done&style=none&taskId=u9134d7a2-0329-478b-a067-a7699bb71eb&title=)
### Step3 -- Stackelberg game
The Stackelberg Game loop:
:::info
while epoch < max_epoch:

1. re-initialize environment with trained agents
2. set environment update mode (leader update or follower update)
3. run environment, export trained agents
4. record & plot results
:::
And the code:
```python
####################
# Stackelberg Game #
####################
# records
stkbg_agents_revenues = []
stkbg_agents_incomes = []
stkbg_platform_rewards = []
# init agents & env
init_stkbg_agents = init_agents_for_stkbg(trained_agents, leading_agent_name, args)
dynamic_env.init_truthful_info()  # initialize information (set truthfully reveal)
# run stackelberg game
cur_agents = init_stkbg_agents
for epoch in range(args.stackelberg_epoch):
    print('Stackelberg Game Epoch:', epoch)
    # re-init env
    reinit_env_for_stkbg(dynamic_env, cur_agents)
    # set env update mode
    set_env_update_mode(dynamic_env, epoch)
    # run env
    platform_reward = dynamic_env.step()
    # record results
    stkbg_platform_rewards.append(platform_reward)
    stkbg_agents_revenues.append(get_agents_revenues(cur_agents, args))
    stkbg_agents_incomes.append(get_agents_incomes(cur_agents, args))
    # plot results
    print('plotting stackelberg results...')
    plot_trained_agents_stkbg(cur_agents, leading_agent_name, args, epoch)

####################
```
**Results (Stackelberg results)**<br />final round leader bid policy![leader_bid_policy_round19.png](https://intranetproxy.alipay.com/skylark/lark/0/2023/png/94556578/1683712906390-2ccf1915-3159-482e-b89f-9cba924e8bcb.png#clientId=u9945c565-baff-4&from=drop&id=ue6353727&originHeight=480&originWidth=640&originalType=binary&ratio=2&rotation=0&showTitle=false&size=32402&status=done&style=none&taskId=u2138391e-277d-4b38-a450-842ba5732c9&title=)<br />final round leader info policy![leader_info_policy_round19.png](https://intranetproxy.alipay.com/skylark/lark/0/2023/png/94556578/1683712920049-da3fbd8d-0f15-433b-b671-397cc758f906.png#clientId=u9945c565-baff-4&from=drop&id=ud099d628&originHeight=480&originWidth=640&originalType=binary&ratio=2&rotation=0&showTitle=false&size=22741&status=done&style=none&taskId=u1f33df41-2ce9-42b7-a579-5bb474e0ea8&title=)<br />final round player 1 policy![player_1_bid_policy_round19.png](https://intranetproxy.alipay.com/skylark/lark/0/2023/png/94556578/1683712929422-83704e8e-cba5-4b6c-b7d7-324f330b7924.png#clientId=u9945c565-baff-4&from=drop&id=u7ce07d1e&originHeight=480&originWidth=640&originalType=binary&ratio=2&rotation=0&showTitle=false&size=35060&status=done&style=none&taskId=u38f494c4-87d4-4f08-84f6-81af746afa3&title=)<br />final round player 2 policy![player_2_bid_policy_round19.png](https://intranetproxy.alipay.com/skylark/lark/0/2023/png/94556578/1683712931962-b3fd1214-dee7-470f-8b08-ea77d5c8c5a5.png#clientId=u9945c565-baff-4&from=drop&id=ud145ac1c&originHeight=480&originWidth=640&originalType=binary&ratio=2&rotation=0&showTitle=false&size=37868&status=done&style=none&taskId=u6e7435a0-4d3f-4e8f-8285-7d120695298&title=)<br />leader income per round![leader_income_per_epoch_stkbg.png](https://intranetproxy.alipay.com/skylark/lark/0/2023/png/94556578/1683712950625-0efd2dd7-5ab3-4ab0-a348-c757c28efeaa.png#clientId=u9945c565-baff-4&from=drop&id=u3d806c7b&originHeight=480&originWidth=640&originalType=binary&ratio=2&rotation=0&showTitle=false&size=34762&status=done&style=none&taskId=ud97cb491-3b30-432e-9011-f739ee52e55&title=)	<br />follwer revenue per round![follwer_income_per_epoch_stkbg.png](https://intranetproxy.alipay.com/skylark/lark/0/2023/png/94556578/1683712946632-d4f6dfbd-63bc-467a-a587-ea1375af8809.png#clientId=u9945c565-baff-4&from=drop&id=u0b3d2796&originHeight=480&originWidth=640&originalType=binary&ratio=2&rotation=0&showTitle=false&size=48151&status=done&style=none&taskId=uefa0bd9a-769d-4738-882e-80d41e30efb&title=)<br />sell price per round![sell_price_per_epoch_stkbg.png](https://intranetproxy.alipay.com/skylark/lark/0/2023/png/94556578/1683712939473-5c7024ed-db80-4b60-8ca1-b0b7f2fcd29d.png#clientId=u9945c565-baff-4&from=drop&id=ud44b9fab&originHeight=480&originWidth=640&originalType=binary&ratio=2&rotation=0&showTitle=false&size=34483&status=done&style=none&taskId=u378432b9-48c3-4638-a009-b208d1e0651&title=)

---


## Code Details
#### Args for the setting
The example game is simulated as: 3 bidders; cost i.i.d. ~ Uniform[0,10); bidding range: [0,20), discount range: [1,10), information signal range: [0,10).<br />Parameters of this example problem are as follows: 

| **Args** | **Explanation** |
| --- | --- |
| `arg.player_num = 3` | 3 bidders |
| `args.bidding_range = 20` | bidding range is [0,20) |
| `args.valuation_range = 10` | valuation(cost) range is [0,10) |
| `args.allocation_mode = 'lowest'` | allocation mode: lowest bidder wins |
| `args.discount_range = 10 - 1` | discount range is [1,10) |
| `args.info_signal_range = args.discount_range + 1`<br />`args.info_algo_range = args.discount_range` | Here we set information signal in [0,10), where signal=0 means no information shared, and [1,10) represents signals revealed by player 0 (i.e. 'info_algo_range'), since truthfully revealed signal about discount has the same range as discount.<br />So 'info_algo_range' is set with 'discount_range', while 'info_signal_range' is set with 'discount_range' + 1 |
| `args.exp_id = 0`<br />`args.folder_name = 'stackelberg'` | Experiment results will be saved in './results/{folder_name}/', and exp_id is used to identify different exp setting in the same folder. |
| `args.stackelberg_epoch` | rounds of stackelberg game |
| `args.env_iters ` | number of iterations in one round (i.e. 'env.step()' in the code) |
| `args.estimate_frequent` | estimate policy results per {estimate_frequent} iters |
| `args.revenue_averaged_stamp ` | platform revenue is record and averaged per {revenue_averaged_stamp} iters |
| `args.exploration_epoch` | number of exploration iters for agent's algorithm |
| `args.step_floor` | epsilon decay parameter |

or check the code:
```python
def easy_signal_setting(args, mode='default'):
    args.mechanism = 'customed'
    args.player_num = 3

    args.overbid = True  # overbid: bid can exceed value(sell price >= cost)
    args.bidding_range = 20  # bid range [0,20)
    args.valuation_range = 10  # valuation range is cost range here [0,10)
    args.allocation_mode = 'lowest'  # allocate to bidder with the lowest price

    args.discount_range = 10 - 1  # discount [1,10)
    # There are two type of information:
    # info_signal = 0 => no information is revealed
    # info_signal: [1,10) => player 0 reveal information (such as truthfully reveal discount)
    args.info_signal_range = args.discount_range + 1  # info signal range [0,10)
    args.info_algo_range = args.discount_range  # info algo range is revealed information -> [1,10)

    args.exp_id = 0  # experiment id (for result saving)
    args.folder_name = 'stackelberg'  # result folder name

    args.stackelberg_epoch = 4 * 5  # stackelberg game rounds
    args.env_iters = 80 * 10000  # environment iterations
    args.estimate_frequent = 40 * 10000  # estimate per K iters
    args.revenue_averaged_stamp = 10000
    args.exploration_epoch = 20 * 10000
    args.step_floor = 4 * 10000

    args.round = 10

    return args
```
### <br />

#### Function 
Several functions used in `stackelberg_game_example.py` for game setting:

| **Function** | **Explanation** |
| --- | --- |
| `dynamic_env.turnon(off)_follower_agent_update` | turn on/off follwer bid policy update |
| `dynamic_env.turnon(off)_leading_agent_update` | turn on/off leader bid policy update |
| `dynamic_env.turnon(off)_leader_discount_update` | turn on/off leader discount policy  |
| `dynamic_env.turnon(off)_leader_info_update` | turn on/off leader information policy update |
| `dynamic_env.enable(disable)_discount` | enable/disable leader discount (price reduction) |
| `dynamic_env.enable(disable)_information` | enable/disable leader information (whether to reveal signal) |
| `dynamic_env.export_all_agent` | export trained agents in dynamic env, agents are stored as dict{name:agt} |
| `dynamic_env.set_rl_agent` | set a RL agent with its agent_name for dynamic_env |
| `dynamic_env.step` | run the auction environment according to parameters set already, it will return latest averaged platform reward (here platform reward refers to market purchase). |
| `dynamic_env.re_init` | re-initialize the auction environment, for different setting(such as enable/disable information) |
| `dynamic_env.init_truthful_info ` | initialize information with truthfully reveal |
| `dynamic_env.adjust_reward_mode` | adjust reward mode for leader |
| `set_leader_algorithm` | default agent donnot have discount or information reveal method, this function can bind discount/signaling method for leader agent  |
| `new_agent.reset_algo` | reset agent's bid policy algorithm (optional) |

Several used functions for Stackelberg game in `stackelberg_game_example.py`:

| **Function** | **Explanation** |
| --- | --- |
| `init_agents_for_stkbg ` | generate initial agents for stackelberg game training |
| `reinit_env_for_stkbg ` | re-initialize environment with trained_agents, so that different update mode can be set |
| `set_env_update_mode ` | set environment update mode (leader update or follower update) |
| `plot_trained_agents_stkbg  ` | plot trained agents in stackelberg game |

