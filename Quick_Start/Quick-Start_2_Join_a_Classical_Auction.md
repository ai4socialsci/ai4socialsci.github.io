### Join different types of auctions 
As introduced with multiple supported scenarios, we implement multiple mini-envs for an easy_start, where you can directly import different mini-envs from "/mini_env/ " folders and modify the auction entrance.<br />[Liuyi: We provide multiple mini-envs for an easy start, and you can directly import different mini-envs from "/mini_env/ " folders and modify the auction entrance]<br />[Liuyi: what does "modify the auction entrance" mean?]

#### Mini-env1: Sealed Value-based auction
In the first example ("./easy_start_agent_policy.py"), the platform hold a seal value-based auction. The initialization starts by the following codes.<br />[Liuyi: "The initialization starts by the following codes." -> The mini-env is initialized by the following code.  ]<br />[Liuyi: P.S., in generally, the class name usually in uppercase~]
```python
    dynamic_env = simple_mini_env1(round=args.round, item_num=args.item_num, mini_env_iters=10000,
                                   agent_num=args.player_num, args=args,
                                   policy=Policy(allocation_mode=args.allocation_mode, 
                                                 payment_mode=args.mechanism,
                                                 action_mode=args.action_mode, 
                                                 args=args),
                                   set_env=None,
                                   init_from_args=True, #init from args rather than input params ->
        )

```

The detailed auction format definition is located in  ("/mini_env/simple_mini_env1.py"), we also plot a summarized functionality in the following plot.<br />![image.png](https://intranetproxy.alipay.com/skylark/lark/0/2023/png/229273/1681285646960-b00ed217-72c5-45ab-b40a-65a0bd83654e.png#clientId=u05dcf58c-1745-4&from=paste&height=254&id=ua837d6f4&originHeight=254&originWidth=699&originalType=binary&ratio=1&rotation=0&showTitle=false&size=115663&status=done&style=none&taskId=u785c9f27-9b21-4557-9de9-bab3c3a8f5f&title=&width=699)<br />Fig. Value-based Mini-env Setting 

Other configure modification such as "switch to First price" , "adjust bidding / valuation range" can be modified in the parameters and we give the following description.

##### Custom configuration 
We have implement multiple configurations for easy modification towards the auctions, including agent number, item valuation range, etc.<br />We show several examples to customize an auction. You can find more on the configuration


###### involoved agent number 
The simplest way to adjust the agent number is to assign different number in the configuration by
```shell
python test_signal_game.py --player_num 4
```

[Liuyi: "The simplest way to adjust the agent number is to assign different number in the configuration by" -> The simplest way to adjust the agent number is by changing the value of --player_num in configuration.]<br />[Liuyi: not sure whether to provide the code with example of test_signal_game.py?]<br />Another way to dynamic adjust the agent number is to set agent number through "set_rl_agent" and assign a empty agent by agent name. Default, agent name rule is "player_x" start from 0. 

```python
# adjust agent policy (include three metioned method)：
dynamic_env.set_rl_agent(rl_agent=Empty_agent,
                         agent_name=custom_agent_name) #adjust agent type
```

###### item valuation distrbution
In the example, we apply Uniform distritbuion with minimal bid (mimimal bid 1 unit), and we use discret space. 

The simplest way to adjust item distribution is through the following code:
```python
python test_signal_game.py --valuation_range 30
```

We also implement multiple distributions rather than Uniform valuation or directly invlove correlated valuation distribution.<br /> [Liuyi: How to call the already implemented distribution? through which hyper-parameter?]

The assignment in the example is through simple_mini_env.py func as following.<br />[Liuyi: "The assignment in the example is through simple_mini_env.py func as following. -> "The assignment of the uniformation distribution in the provided example is through assign_generate_true_value method defined in simple_mini_env.py as following.]
```python
    def assign_generate_true_value(self, agent_name_list):

        def generate_true_value(self):
            true_value = random.randint(self.lowest_value, self.highest_value)
            self.record_true_value(true_value)

        for name in agent_name_list:
            bind(self.agt_list[name], generate_true_value)
            print(f'bind (function) generate_true_value for agent {name}')
```

In other words, we can re-write custom valuation generate value function and call the assignment from mini_env.<br />[Liuyi: It seems that if the user want to use the customized dist, they need to replace the func generate_true_value to their own? If so: "In other words, we can re-write custom valuation generate value function and call the assignment from mini_env." -> In other words, we can re-write the generate_true_value function to the customized generate value function.]

###### Information reveal 
In the sealed auction, normally only the winner know the payment and item value. However, we can also evaluate different information reveal effects. 

We by default set the "winner_only" parameters, and when it sets to "False" all the bidders will receive the payment and item value. The detailed realization follows in the mini-env (e.g. simple_mini_env.py) definition. <br />[Liuyi: "However, we can also evaluate different information reveal effects. We by default set the "winner_only" parameters, and when it sets to "False" all the bidders will receive the payment and item value. " -> Our platform also enable the evaluation of different information reveal settings through the "winner_only" parameter. When set it to "False", all the bidders will receive the payment and item value. In mini_env/simple_mini_env1.py, it is realized by calling increase_info_to_obs function]
```python
        if not self.winner_only or allocation:
                # add the information of the payment into the obs
                obs = increase_info_to_obs(obs, extra_info_name='payment', value=reward)
```

Therefore,  you can custom any information reveal to any agent observation through the function "increase_info_to_obs" by filtering agent name or other condition. <br />[Liuyi: how to filtering agent name or other condition in function increase_info_to_obs?]




---

#### Mini-env2: Sealed Signal-based auction
We also introduce another example using mini-env2 (signal based auction), you can run the following example through the command. In the example, different type of agents are invloved including agent with Epsilon greedy policy or DQN policy build by Pytorch.<br />[Liuyi: mini-env2 not included in the code (mini_env_2_a.py or mini_env_2_b.py)]
```shell
cd test_mini_env
python test_signal_game.py
```

The mini env definition is in the "/mini_env/base_signal_mini_env.py"
```python
    dynamic_env = signal_mini_env(round=args.round, item_num=args.item_num, mini_env_iters=10000,
                                   agent_num=args.player_num, args=args,
                                   policy=Policy(allocation_mode=args.allocation_mode, 
                                                 payment_mode=args.mechanism,
                                                 action_mode=args.action_mode, 
                                                 args=args),
                                   set_env=None,
                                   init_from_args=True, #init from args rather than input params ->
                                  private_signal_generator=None,
                                  signal_type='normal',winner_only=only_winner_know_true_value
        )

```

We also plot a summarized functionality in the following plot.<br />[Liuyi: The figure seems not too much different with the figure in section Mini-env1: Sealed Value-based auction? ]

![image.png](https://intranetproxy.alipay.com/skylark/lark/0/2023/png/229273/1681286556050-4a1f1e27-fa35-4b98-8bff-ab282b435b42.png#clientId=u05dcf58c-1745-4&from=paste&height=316&id=ucd79de05&originHeight=316&originWidth=755&originalType=binary&ratio=1&rotation=0&showTitle=false&size=155873&status=done&style=none&taskId=u659a2a12-fc5c-40f3-9611-78ff1a60f9e&title=&width=755)<br />Fig. Signal-based Mini-env Setting 

Other configure modification such as "switch to First price" , "adjust bidding / valuation range" can be modified in the parameters(args config) 

---

#### Mini-env3: Open Signal-based auction

We introduce a classic opening auction APA (Ascending Price Auction) as our examples where users sequentially submit their bid after witness their valuation (or signal) and the current highest bid. In our examples, we show a greedy agent who will bid between the current highest bid and their computed maximal bid upbound. The upbound is computed by the RL algorithm and we take iid private signal or the public signal as example.

The datailed example is located in "/test_mini_env/test_open_game.py" .
```shell
cd ./test_mini_env/
python test_open_game.py
```

If users observe the same public signal, after learning, they will bid slightly lower than their valuation on the First price mechanism. <br />[Liuyi: The following figure is the results by running the above command. It shows that tf users observe the same public signal, after learning, they will bid slightly lower than their valuation on the First price mechanism. ]<br />![agt_4_no_overbid_first_price_epoch_400000.png](https://intranetproxy.alipay.com/skylark/lark/0/2023/png/349354/1684313707334-0df45760-4947-41c0-8289-039f7e7d9488.png#clientId=u7200d1c6-34cc-4&from=drop&height=311&id=u9a90c18a&originHeight=480&originWidth=640&originalType=binary&ratio=2.5&rotation=0&showTitle=false&size=24373&status=done&style=none&taskId=u6014ab72-9afa-4d54-b3ba-aaa844cb0b7&title=&width=415)<br />![image.png](https://intranetproxy.alipay.com/skylark/lark/0/2023/png/229273/1682066708944-31330213-acd3-42ae-84b9-3eb23f40525a.png?x-oss-process=image/format,png#clientId=u12d3e65b-5900-4&from=paste&height=423&id=u6a57b697&originHeight=480&originWidth=640&originalType=binary&ratio=1&rotation=0&showTitle=false&size=1229090&status=done&style=none&taskId=u4d34ae46-0183-4df7-bae4-f231263105a&title=&width=564) <br />Fig. Agent Policy on Open Ascending First Price Auction 

The detailed open auction env is define in the "/mini_env/open_auction.py".
```python
dynamic_env = open_ascending_auction(round=args.round, item_num=args.item_num, mini_env_iters=10000,
                                         agent_num=args.player_num, args=args,
                                         policy=Policy(allocation_mode=args.allocation_mode,
                                                       payment_mode=args.mechanism,
                                                       action_mode=args.action_mode,
                                                       args=args),
                                         set_env=None,
                                         init_from_args=True,  # init from args rather than input params ->
                                         private_signal_generator=None,
                                         signal_type='normal', winner_only=only_winner_know_true_value,
                                         max_bidding_times=args.max_bidding_times, minimal_raise=args.minimal_raise

                                         )
```


Other configure modification such as "switch to Second price" , "adjust signal / valuation range" can be modified in the parameters(args config)  and we also show detailed config switches in the "/test_mini_env/test_open_game.py".
```python
    if 'private' in mode:
        args.private_signal=1
        args.private_signal_range=10
      
#--- see code 381 set private signal only  ----
  		dynamic_env.set_public_signal_generator(None)
        
        private_signal_generator = base_private_Signal_generator(
            dtype='Discrete', default_feature_dim=args.private_signal_dim,
            default_lower_bound=0,
            default_upper_bound=args.private_signal_range,
            default_generation_method='uniform',
            default_value_to_signal=0,
            agent_name_list=dynamic_env.agent_name_list
        )
        private_signal_generator.generate_default_iid_signal()

        dynamic_env.set_private_signal_generator(private_signal_generator)

	
```


---

