## Introduction -- Stackelberg game example setting
### Background
There are 3 suppliers competing with each other by selling identical products. At each round, every supplier receives its cost and bids a price. Who bids the lowest price will win the opportunity to sell its product at the price it bids.<br />At first, everyone seeks to maximize its own revenue, i.e. (price - cost) * allocation. Then player 0 decides to reduce its price to seize the market, by maximizing its income, i.e. price * allocation.
### Experiment Setting
In a repeated single-round auction environment, 3 bidders' costs are denoted as ci ~ Fi respectively. At each round, every bidder observes its cost ci bids a price according to its policy: bi=π(ci), then receive a reward (revenue or income), depending on whether they win the auction and how much they have bid. <br />To illustrate the evolution of equilibrium when agents behaves under different targets and show the effect of signal in such a Stackelberg game, we conduct 3 experiments as follows.
#### Warmup 1 -- Symmetric Competition
In this setting, every bidder maximize its revenue, which will result in a symmetric equilibrium.
#### Warmup 2 -- Simplified Stackelberg Game
In this setting, player 0 decides to reduce its price according to a discount policy discout=πd(ci) on the basis of previous equilibirum, in order to maximize its income, and we turn this game into a simplified Stackelberg game, which contains 2 steps as follows:

1. Leader move: the leader (i.e player 0) updates its discount policy, while the other players (the followers) fix their bid policy.
2. Follower response: the leader fixs its discount policy, and the followers update their bid policy.

To compute and approximate a Stackelberg equilibirum, it is required to repeatedly update leader's policy (step1) based on the "best response" of followers(step2). We leave the equilibrium approximation to latter setting, and simpilify the game to observe a preliminary result.<br />Additionally, we also consider information's impact on this simplified Stackelberg game: if the leader reveals a truthful signal before it makes a discount decision, which will be observed by the followers, how will their policies be like? This experiment is coducted in step 3:

3. Leader reveal: the leader truthfully reveal its discount decision, and the followers update a bid policy denoted as bi=π(ci, discount).
#### Experiment 3 -- Stackelberg Game with Signaling
Finally, We put all agents in a full Stackelberg game, where the leader makes discount, reveals signal and maximizes its income, while the followers receive signal and bid to maximize their revenue. <br />We approximate the Stackelberg equilibrium by taking turns updating the policy of leader and followers repeatedly, i.e. trainning with the following 2 steps in a loop:<br />while epoch < max_epoch:

1. Leader update its policy(discount policy or signal policy), follwers fix policy;
2. Leader fixs its policy, followers update.

![Stackelberg game approximation](https://intranetproxy.alipay.com/skylark/lark/0/2023/png/94556578/1684405369875-87512454-e4bd-400d-8e97-51100bc4719f.png#clientId=u43244ba4-00b2-4&from=drop&height=182&id=u22aa677c&originHeight=792&originWidth=1370&originalType=binary&ratio=2&rotation=0&showTitle=true&size=71879&status=done&style=stroke&taskId=uf4900975-8f68-4ff1-a473-f4732d261d4&title=Stackelberg%20game%20approximation&width=315 "Stackelberg game approximation")

---

## Implement with AI4SC platform
### Run with command
In order to run the example game above, please run:
> cd Stackelberg_Game
> python3 stackelberg_game_example.py

The results will be plotted and saved in "Stackelberg_Game/results/stackelberg/", and the numeric results will be printed in logs.
### Warmup1 -- Symmetric Competition
#### Setting 
In this basic setting, everyone updates its bid policy to maximize its revenue, and no one will make discount or reveal information. 
#### Code
Set the environment according to our setting, and run`env.step()`:
> For simplicity of the code, even though we donnot require the definition of leader/follower in this setting, the environment still identifies the player 0 as the leader and the others as the followers, but this doesn't affect the code's function as a simulation of warmup1 setting.

```python
# Warmup-1
# everyone update bid policy, no discount, no information
dynamic_env.turnon_follower_agent_update()
dynamic_env.turnon_leading_agent_update()
dynamic_env.disable_discount()
dynamic_env.disable_information()

platform_reward_r0 = dynamic_env.step()  # run the mini env and update policy

# export agent algo
trained_agents = dynamic_env.export_all_agent()
```
#### Theoretical Results 
This setting is symmetric among 3 players, so they should converge to a symmetric equilibrium.
#### Experiment Results
The bidding policy of 3 bidders (bid value given different cost):<br />![Fig 1.1: bidding policy in warmup 1](https://intranetproxy.alipay.com/skylark/lark/0/2023/png/94556578/1684730016982-73469411-c8fb-4650-9702-632b01e74c62.png#clientId=u9a502d67-47c7-4&from=drop&height=330&id=u3e8ad174&originHeight=480&originWidth=640&originalType=binary&ratio=2&rotation=0&showTitle=true&size=25332&status=done&style=stroke&taskId=ua9464643-1389-4c5e-a8d2-7ab77d02073&title=Fig%201.1%3A%20bidding%20policy%20in%20warmup%201&width=440 "Fig 1.1: bidding policy in warmup 1")<br />The evolution of player 0's revenue during the training steps:<br />![Fig 1.2: player 0 revenue curve in warmup 1](https://intranetproxy.alipay.com/skylark/lark/0/2023/png/94556578/1684730098569-eaa23fc8-f32f-4232-9ad6-7627de2a35e6.png#clientId=u9a502d67-47c7-4&from=drop&height=330&id=u370b794f&originHeight=480&originWidth=640&originalType=binary&ratio=2&rotation=0&showTitle=true&size=61168&status=done&style=stroke&taskId=ubc12f23e-613c-42a2-b75c-bb73d449c25&title=Fig%201.2%3A%20player%200%20revenue%20curve%20in%20warmup%201&width=440 "Fig 1.2: player 0 revenue curve in warmup 1")<br />The evolution of player 1 and player 2's revenue during the training steps:<br />![Fig 1.3: player 1&2 revenue curve in warmup 1](https://intranetproxy.alipay.com/skylark/lark/0/2023/png/94556578/1684730101259-1dadd8b8-da0e-425c-be0c-3d5fb1a14763.png#clientId=u9a502d67-47c7-4&from=drop&height=330&id=u271279a2&originHeight=480&originWidth=640&originalType=binary&ratio=2&rotation=0&showTitle=true&size=66306&status=done&style=stroke&taskId=u42f1f324-b2c9-4ebd-beab-2a84210b800&title=Fig%201.3%3A%20player%201%262%20revenue%20curve%20in%20warmup%201&width=440 "Fig 1.3: player 1&2 revenue curve in warmup 1")
#### Analysis
As we can see in Fig1.1, the bidding policy of 3 different players are quite similar, and Fig1.2 & Fig1.3 shows that the revenue of every player converges to around 0.92.
#### Conclusion
The similarity of bidding policy and symmetry of revenue among 3 bidders shows that they converges to a symmetric equilibrium.

---

### Warmup2 -- Simplified Stackelberg Game
#### Overview
This simplified Stackelberg game contains the following 3 steps:

1. Leader move: the leader (i.e player 0) updates its discount policy to maximize income, while the other players (the followers) fix their bid policy.
2. Follower response: the leader fixs its discount policy, and the followers update their bid policy.
3. Leader reveal: the leader truthfully reveal its discount decision, and the followers update a bid policy denoted as bi=π(ci, discount).
#### **Step 1: Leader moves, followers fix policy**
##### Setting
In this step, the leader moves, updating its discount policy while fixing its bid policy (which is trained in the previous symmetric game), and the followers should fix their policy. The envrionment should enable discount, and information is still disabled.
##### Code
After set the environment based on step 1 setting, we need to copy previously trained agents and set them to environment (by`set_rl_agent`), and augment leader agent with discount/info policy(which is not required in preivous game) using`set_leader_algorithm`. Then just run env and save results:
```python
# Warmup-2-1  decrease + others not update

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
##### Experiment Results
The bidding policy of 3 bidders (bid value given different cost):<br />![Fig 2.1.1: bidding policy in warmup 2, step 1](https://intranetproxy.alipay.com/skylark/lark/0/2023/png/94556578/1684730190463-ce5e4bc5-3b0b-4c08-8f01-e5812d3fcef7.png#clientId=u9a502d67-47c7-4&from=drop&height=330&id=u99e83ab9&originHeight=480&originWidth=640&originalType=binary&ratio=2&rotation=0&showTitle=true&size=31866&status=done&style=stroke&taskId=u9b1c1728-098d-44aa-a5d9-40a0dc0025a&title=Fig%202.1.1%3A%20bidding%20policy%20in%20warmup%202%2C%20step%201&width=440 "Fig 2.1.1: bidding policy in warmup 2, step 1")<br />The evolution of leader income during traininig steps:<br />![Fig 2.1.2: leader income curve in warmup 2, step 2](https://intranetproxy.alipay.com/skylark/lark/0/2023/png/94556578/1684730212616-f35737dd-81b5-4a8c-88da-5b9de70c376f.png#clientId=u9a502d67-47c7-4&from=drop&height=330&id=u761e06aa&originHeight=480&originWidth=640&originalType=binary&ratio=2&rotation=0&showTitle=true&size=71121&status=done&style=stroke&taskId=u8340af2f-fafd-4135-a225-79e76afca98&title=Fig%202.1.2%3A%20leader%20income%20curve%20in%20warmup%202%2C%20step%202&width=440 "Fig 2.1.2: leader income curve in warmup 2, step 2")<br />The evolution of followers income during training steps:<br />![Fig 2.1.3: follower revenue curve in warmup 2, step 3](https://intranetproxy.alipay.com/skylark/lark/0/2023/png/94556578/1684730244399-70f58460-d6de-4e10-beb4-91a363ae7432.png#clientId=u9a502d67-47c7-4&from=drop&height=330&id=uefc9580f&originHeight=480&originWidth=640&originalType=binary&ratio=2&rotation=0&showTitle=true&size=99087&status=done&style=stroke&taskId=u61061eb4-7193-4e7a-be73-68bca98bb42&title=Fig%202.1.3%3A%20follower%20revenue%20curve%20in%20warmup%202%2C%20step%203&width=440 "Fig 2.1.3: follower revenue curve in warmup 2, step 3")
##### Analysis
In Fig2.1.1, we can see that the leader has reduced its price based on its discount policy; in Fig 2.1.2, we can see that the leader's income converges to around 2.15, while in Fig 2.1.3 we can see that the followers' revenue converges to around 0.60.
##### Conclusion 
By price reduction, the leader has acquired great income, while the followers' revenue decreases becaus of the leader has seized the market.
#### Step 2: Followers response, leader fixes policy
##### Setting
In this step, the followers compute their best response according to the leader's decision, so they need to update their bid policy, while the leader should fix both its bid policy and discount policy. The environment should enable discount and still disable information.
##### Code
Set environment according step2's setting, then copy trained agents in previous step, and run env, save results:
```python
# Warmup-2-2 --- follower update

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
##### Experiment Results
The bidding policy of 3 bidders (bid value given different cost):<br />![Fig 2.2.1: bidding policy in warmup2 step2](https://intranetproxy.alipay.com/skylark/lark/0/2023/png/94556578/1684730317010-d676e71d-4bfe-40ac-a8d3-a815dcc2f246.png#clientId=u9a502d67-47c7-4&from=drop&height=330&id=uc334d350&originHeight=480&originWidth=640&originalType=binary&ratio=2&rotation=0&showTitle=true&size=28625&status=done&style=stroke&taskId=u5e6e5199-34b7-4d25-aaa4-24050f1c83f&title=Fig%202.2.1%3A%20bidding%20policy%20in%20warmup2%20step2&width=440 "Fig 2.2.1: bidding policy in warmup2 step2")<br />The evolution of leader's income during trainning steps:<br />![Fig 2.2.2: leader income curve in warmup2 step2](https://intranetproxy.alipay.com/skylark/lark/0/2023/png/94556578/1684730322797-a07e1800-20a8-4f5d-9443-1375708740f0.png#clientId=u9a502d67-47c7-4&from=drop&height=330&id=ue675dae3&originHeight=480&originWidth=640&originalType=binary&ratio=2&rotation=0&showTitle=true&size=64345&status=done&style=stroke&taskId=ub6d876e6-7f21-461c-af82-12c6f440244&title=Fig%202.2.2%3A%20leader%20income%20curve%20in%20warmup2%20step2&width=440 "Fig 2.2.2: leader income curve in warmup2 step2")<br />The evolution of followers' revenue during training steps:<br />![Fig 2.2.3: followes revenue curve in warmup2 step2](https://intranetproxy.alipay.com/skylark/lark/0/2023/png/94556578/1684730330045-3036a274-9ef7-46c7-9a47-27adb85271f6.png#clientId=u9a502d67-47c7-4&from=drop&height=330&id=u3b2d56d0&originHeight=480&originWidth=640&originalType=binary&ratio=2&rotation=0&showTitle=true&size=82709&status=done&style=stroke&taskId=u278b49da-b7f9-470d-9c6d-0cb812c317c&title=Fig%202.2.3%3A%20followes%20revenue%20curve%20in%20warmup2%20step2&width=440 "Fig 2.2.3: followes revenue curve in warmup2 step2")
##### Analysis
As we can see in Fig 2.2.1, the bidding price of both leader and followers has reduced. Comparing with step1, the income of the leader has decreased, while the revenue of the followers has slightly increased. 
##### Conclusion
By responsing to the leader's policy, the followers can optimize their decision and gain more revenue.
#### Step 3: Leader fixes policy and truthful reveal, followers response
##### Setting
In this setting, we require the leader to truthfully reveal its discount decision, and the followers will see it and update a bid policy denoted as bi=π(ci, discount). The leader's bid and discount policy are still fixed, and the environment should enable both discount and information.
##### Code
Set the environment with step 3 setting, then copy trained agents in step1, and run env, save results:
```python
# Warmup-2-3 --- information reveal

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
##### Experiment Results
The bidding policy of the leader (bid value/discount given different cost):<br />![Fig 2.3.1: leader's bidding policy in warmup2 step3](https://intranetproxy.alipay.com/skylark/lark/0/2023/png/94556578/1684730448771-66522173-5058-47ed-8afa-5e00320ac661.png#clientId=u9a502d67-47c7-4&from=drop&height=330&id=u9fb51ec7&originHeight=480&originWidth=640&originalType=binary&ratio=2&rotation=0&showTitle=true&size=27461&status=done&style=stroke&taskId=uf9e67a7a-3796-40e6-8597-8e66b5aa4dc&title=Fig%202.3.1%3A%20leader%27s%20bidding%20policy%20in%20warmup2%20step3&width=440 "Fig 2.3.1: leader's bidding policy in warmup2 step3")<br />The bidding policy of follower 1 (bid value given different cost and leader information):<br />![Fig 2.3.2: follower 1's bidding policy in warmup2 step3](https://intranetproxy.alipay.com/skylark/lark/0/2023/png/94556578/1684730499548-01de8b96-07d7-4113-a059-646b13fd5f96.png#clientId=u9a502d67-47c7-4&from=drop&height=330&id=u3a556636&originHeight=480&originWidth=640&originalType=binary&ratio=2&rotation=0&showTitle=true&size=29389&status=done&style=stroke&taskId=uab012d05-1dfa-4cfb-aaaa-921605e9d62&title=Fig%202.3.2%3A%20follower%201%27s%20bidding%20policy%20in%20warmup2%20step3&width=440 "Fig 2.3.2: follower 1's bidding policy in warmup2 step3")<br />The bidding policy of follower 2 (bid value given different cost and leader information):<br />![Fig 2.3.3: follower 2's bidding policy in warmup2 step3](https://intranetproxy.alipay.com/skylark/lark/0/2023/png/94556578/1684730518067-444e29a6-b220-4957-b99e-1dc6c63aaa11.png#clientId=u9a502d67-47c7-4&from=drop&height=330&id=ud4fecb5e&originHeight=480&originWidth=640&originalType=binary&ratio=2&rotation=0&showTitle=true&size=30798&status=done&style=stroke&taskId=u8fe91155-eae3-409c-9e58-913357b64ae&title=Fig%202.3.3%3A%20follower%202%27s%20bidding%20policy%20in%20warmup2%20step3&width=440 "Fig 2.3.3: follower 2's bidding policy in warmup2 step3")<br />The evolution of leader's income during trainning steps:<br />![Fig 2.3.4: leader income curve in warmup2 step3](https://intranetproxy.alipay.com/skylark/lark/0/2023/png/94556578/1684730532287-cba2a490-6b7f-480c-be20-fa8c2e007224.png#clientId=u9a502d67-47c7-4&from=drop&height=330&id=uc62da11e&originHeight=480&originWidth=640&originalType=binary&ratio=2&rotation=0&showTitle=true&size=71200&status=done&style=stroke&taskId=u9c69c5c6-b545-46a8-b39d-7354b28d159&title=Fig%202.3.4%3A%20leader%20income%20curve%20in%20warmup2%20step3&width=440 "Fig 2.3.4: leader income curve in warmup2 step3")<br />The evolution of followers' revenue during trainning steps:<br />![Fig 2.3.4: follower revenue curve in warmup2 step3](https://intranetproxy.alipay.com/skylark/lark/0/2023/png/94556578/1684730547081-52b05e47-706a-4e93-9307-f8787a0bd89f.png#clientId=u9a502d67-47c7-4&from=drop&height=330&id=u43a592ea&originHeight=480&originWidth=640&originalType=binary&ratio=2&rotation=0&showTitle=true&size=92619&status=done&style=stroke&taskId=ufdaa5036-0d57-4559-917d-562a77769f6&title=Fig%202.3.4%3A%20follower%20revenue%20curve%20in%20warmup2%20step3&width=440 "Fig 2.3.4: follower revenue curve in warmup2 step3")
##### Analysis
As we can see in Fig2.3.1, the leader chooses discount value at 1 or 2, and when he truthfully reveals these information to the followers, they will show different behavior under different information and cost in Figs 2.3.2 & Fig 2.3.3.<br />From Fig 2.3.4 & Figs 2.3.5, we can see that the income of leader keeps around 1.7 while the revenue of the followers converges to around 0.64, so the leader's income has decreased and the followers revenue has increased, comparing with the results in previous steps.
##### Conclusion
By knowing the truthful information of leader's discount policy, followers can gain more revenue and the leader's income will severely decrease. Obviously truthful telling is not a good signaling policy for the leader.

---

### Experiment 3 -- Stackelberg Game with Signaling
#### Setting
In this Stackelberg game setting, the leader should makes discount, reveals signal to maximize its income, while the followers receive signal and bid to maximize their revenue. <br />The Stackelberg equilibrium is approximated with the following loop:<br />while epoch < max_epoch:

1. Leader update its policy(discount policy or signal policy), follwers fix policy;
2. Leader fixs its policy, followers update.
#### Code
In order to run a Stackelberg Game loop described above, we need to re-initialize the envrionment to assign trained agents to it, and set different update rule (leader or follower update) in each loop, then we can run the environment and save result:
:::info
while epoch < max_epoch:

1. re-initialize environment with trained agents
2. set environment update mode (leader update or follower update)
3. run environment, export trained agents
4. record & plot results
:::
And the code:
```python
##############################
# EXP-3 --- Stackelberg Game #
##############################
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
#### Experiment Results
Final round leader bidding policy (bid & discount value given different costs):<br />![Fig 3.1: leader's bidding policy in exp3](https://intranetproxy.alipay.com/skylark/lark/0/2023/png/94556578/1684730595720-cc241956-cf4c-488c-9295-b48aff2d4af2.png#clientId=u9a502d67-47c7-4&from=drop&height=330&id=uc6a4fa38&originHeight=480&originWidth=640&originalType=binary&ratio=2&rotation=0&showTitle=true&size=30784&status=done&style=stroke&taskId=udd59e7f3-d323-499c-b2b3-41d3ca187b8&title=Fig%203.1%3A%20leader%27s%20bidding%20policy%20in%20exp3&width=440 "Fig 3.1: leader's bidding policy in exp3")<br />Final round leader info policy given different cost (signaling scheme):<br />![Fig 3.2: leader's info(signaling) policy in exp3](https://intranetproxy.alipay.com/skylark/lark/0/2023/png/94556578/1684730602181-4f743d22-b50d-48fa-826e-700c4635f993.png#clientId=u9a502d67-47c7-4&from=drop&height=330&id=u34c5d558&originHeight=480&originWidth=640&originalType=binary&ratio=2&rotation=0&showTitle=true&size=25630&status=done&style=stroke&taskId=uadaaa4bf-2611-436b-b8a6-a473d935aaa&title=Fig%203.2%3A%20leader%27s%20info%28signaling%29%20policy%20in%20exp3&width=440 "Fig 3.2: leader's info(signaling) policy in exp3")<br />Final round follower 1 bidding policy (bid value given different costs):<br />![Fig 3.3: follower 1's bidding policy in exp3](https://intranetproxy.alipay.com/skylark/lark/0/2023/png/94556578/1684730607060-7fec5934-a9af-43f8-ae62-d3bdb7391931.png#clientId=u9a502d67-47c7-4&from=drop&height=330&id=u7b5d23a9&originHeight=480&originWidth=640&originalType=binary&ratio=2&rotation=0&showTitle=true&size=57360&status=done&style=stroke&taskId=ua5067460-faaf-4d3f-8063-99a4a9a8e16&title=Fig%203.3%3A%20follower%201%27s%20bidding%20policy%20in%20exp3&width=440 "Fig 3.3: follower 1's bidding policy in exp3")<br />Final round follower 2 bidding policy (bid value given different costs):<br />![Fig 3.4: follower 2's bidding policy in exp3](https://intranetproxy.alipay.com/skylark/lark/0/2023/png/94556578/1684730612491-421cb038-4eef-409e-a2df-e67a87bcb723.png#clientId=u9a502d67-47c7-4&from=drop&height=330&id=u38af4e4b&originHeight=480&originWidth=640&originalType=binary&ratio=2&rotation=0&showTitle=true&size=59061&status=done&style=stroke&taskId=u14e0b52f-a00a-423c-9fbf-ad2a36b9e35&title=Fig%203.4%3A%20follower%202%27s%20bidding%20policy%20in%20exp3&width=440 "Fig 3.4: follower 2's bidding policy in exp3")<br />Leader income per round:<br />![Fig 3.5: leader's income curve in exp3](https://intranetproxy.alipay.com/skylark/lark/0/2023/png/94556578/1684730621867-1b9a8727-576f-4966-afad-8958eafe4157.png#clientId=u9a502d67-47c7-4&from=drop&height=330&id=uce2fbb67&originHeight=480&originWidth=640&originalType=binary&ratio=2&rotation=0&showTitle=true&size=32594&status=done&style=stroke&taskId=ucc7e148f-bf46-4fe8-8830-57899286198&title=Fig%203.5%3A%20leader%27s%20income%20curve%20in%20exp3&width=440 "Fig 3.5: leader's income curve in exp3")<br />Follower revenue per round:<br />![Fig 3.6: followers' revenue curve in exp3](https://intranetproxy.alipay.com/skylark/lark/0/2023/png/94556578/1684730625161-f48de211-2cc4-4f06-b0e5-525bcb176a59.png#clientId=u9a502d67-47c7-4&from=drop&height=330&id=u5c520c42&originHeight=480&originWidth=640&originalType=binary&ratio=2&rotation=0&showTitle=true&size=39652&status=done&style=stroke&taskId=ub7f3dc8f-b55c-449d-890d-6a022af680d&title=Fig%203.6%3A%20followers%27%20revenue%20curve%20in%20exp3&width=440 "Fig 3.6: followers' revenue curve in exp3")
#### Analysis
From Fig3.1~3.4, we can see that the agents have spontaneously produced complicated signal and bidding behavior, and from Fig 3.5 & Fig 3.6, we can see that the leader's income converges to around 2.2 and the followers' revenue converges to around 0.7 or 0.4. The difference of revenue of the 2 homogenous followers might comes from different initial policy.
#### Conclusion
By conducting a signaling policy, the leader can maximize its income while the follower who can learn a proper response policy can also maximize its revenue, which is in line with the Bayesian persuasion theory.

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
| `args.exp_id = 1`<br />`args.folder_name = 'stackelberg'` | Experiment results will be saved in './results/{folder_name}/', and exp_id is used to identify different exp setting in the same folder. |
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

    args.exp_id = 1  # experiment id (for result saving)
    args.folder_name = 'stackelberg'  # result folder name

    args.stackelberg_epoch = 4 * 5  # stackelberg game rounds
    args.env_iters = 200 * 10000  # environment iterations
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

