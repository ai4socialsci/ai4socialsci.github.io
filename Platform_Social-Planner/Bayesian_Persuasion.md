### Summary 					 				
Suppose one person, call him Sender, wishes to persuade another, call her Receiver, to change her action. The act of sender sending a signal concerning the origin information to the reciver in order to manipulate her action and benifit the sender himself is called persuasion. Bayesian persuasion solves the sufficient and necessary condition of the existence of a siganl that strictly benifit himself, and characterizes the sender-optimal signal if exist[1]. <br />The pipline of a persuasion game is as follows. 

1. Sender decides and public a signaling scheme: ![](https://intranetproxy.alipay.com/skylark/lark/__latex/af0077c46b2c7814a7af6c03a8ccd455.svg#card=math&code=%28S%2C%20%5Cpi%28s%20%5Cmid%20w%29%29%20%5Cforall%20s%20%5Cin%20S%2C%20w%20%5Cin%20%5COmega&id=sPRX7)
2. Nature decides a true state: ![](https://intranetproxy.alipay.com/skylark/lark/__latex/9277d208986a6a96c65e1f59dc248c0e.svg#card=math&code=%5Comega%20%5Cin%20%5COmega&id=aBFyc), Sender and reciever gets a shared prior![](https://intranetproxy.alipay.com/skylark/lark/__latex/f0ae8a7287d2b14898997324b9905c00.svg#card=math&code=%5Cmu_0&id=H1bUh)
3. Sender send a signal![](https://intranetproxy.alipay.com/skylark/lark/__latex/79ce3c7a71877c2ff01695e38ade43ca.svg#card=math&code=s&id=U7kiJ)at the probablity![](https://intranetproxy.alipay.com/skylark/lark/__latex/7979a751618c6c3fd39f7e7c6294e23c.svg#card=math&code=%5Cpi%20%28s%7C%5Comega%29&id=aBlEw)
4. Reciever recieves the signal and take action![](https://intranetproxy.alipay.com/skylark/lark/__latex/5374c9b03f7b24faa014732efa41a695.svg#card=math&code=a%20%5Cin%20A&id=aeZyo)to maximize her expected utility on the postirior distibution![](https://intranetproxy.alipay.com/skylark/lark/__latex/1fb93f39e7d1a63585bf693e4c3ee704.svg#card=math&code=%5Cmu_s&id=zvWif)
5. Sender and Reciver gets their realized utility![](https://intranetproxy.alipay.com/skylark/lark/__latex/5fab9e926c64f7ae363b33ff3e5c4b88.svg#card=math&code=v%28a%2C%20%5Comega%29%2C%20u%28a%2C%20%5Comega%29&id=Ultvz)
### Implement Detail
To simulate a Bayesian persuasion process, we run the process for many epochs to force the Recivers learn the distribution of signal![](https://intranetproxy.alipay.com/skylark/lark/__latex/7979a751618c6c3fd39f7e7c6294e23c.svg#card=math&code=%5Cpi%20%28s%7C%5Comega%29&id=dwrPS)instead of commting it. Once the equilibria is acquired, the platform update the signal to reach the optimal disclosure.<br />![image.png](https://intranetproxy.alipay.com/skylark/lark/0/2023/png/94556582/1684318124328-5a45708c-31d4-44ec-a1e0-e557690287be.png#clientId=u463af090-24ea-4&from=paste&height=604&id=ufa44912b&originHeight=1207&originWidth=1677&originalType=binary&ratio=2&rotation=0&showTitle=false&size=1574378&status=done&style=none&taskId=u569c91e4-33af-4a0f-a6a5-7dfc713b484&title=&width=838.5)<br />We further demostrate how the four stages are conducted in the AI4SS environment as below.
```python
    def generate_signal(self, init=True, data_type='int',upper_bound=None):
        if init:
            self.signal_init()

        if upper_bound is not None:
            cur_upper_bound=upper_bound
        else:
            cur_upper_bound=self.upper_bound

        if self.dtype == "Discrete":
            if self.generation_method == 'uniform':
                for i in range(self.feature_dim):
                    rnd_signal = np.random.uniform(self.lower_bound, cur_upper_bound)  # [low , high]
                    if data_type == 'int':
                        rnd_signal = int(rnd_signal)
                    self.signal_realization[i] = rnd_signal

            if self.generation_method == 'Gaussian':
                for i in range(self.feature_dim):
                    while 1:
                        rnd_signal = np.random.normal(loc=cur_upper_bound/2, scale = 2)
                        if 0 < rnd_signal < cur_upper_bound:
                            break
                    if data_type == 'int':
                        rnd_signal = int(rnd_signal)
                    self.signal_realization[i] = rnd_signal
                    # print(self.signal_realization)
        return
```

1. At the beginning of every epoch of environment updating, the function is called to generate a signal with multiple dimensions on the form of uniform distribution or Gaussian distribution.
```python
    def get_partial_signal_realization(self, observed_dim_list=[]):
        if observed_dim_list is None:
            observed_dim_list = []  # observe None signal

        # when user observe partial signal
        partial_signal = deepcopy(self.signal_realization)
        # for dim in range(self.feature_dim):
        #     if dim not in observed_dim_list:
        #         partial_signal[dim] = None  # not observe the signal
        # obs_signal = []
        # for dim in partial_signal:
        #     if dim is not None:
        #         obs_signal.append(dim)
        a = self.alpha
        # s2 = partial_signal[1]
        s2 = (1-a) * partial_signal[0] + a * partial_signal[1]
        s2 = round(s2)
        partial_signal[1] = s2
        return partial_signal  # the same dim as the whole signal with None
```

2. The signal is generated as shown in the function. In the example we take the linear combination of dimensions to simulate the persuasion game.
```python
            obs = agt.receive_observation(args, self.budget, agent_name, agt_idx,
                                          extra_info=None, observation=observation,
                                          public_signal=public_signal,
                                          # true_value_list=agt.get_true_value_history(),
                                          # action_history=agt.get_action_history(),
                                          # reward_history=agt.get_reward_history(),
                                          # allocation_history=agt.get_allocation_history()
                                          )
            if termination or truncation:
                env.step(None)
            else:
                # print(agent_name, 'agent_name')
                new_action = agt.generate_action(obs)  # get the next round action based on the observed budget
                # agt.record_action(new_action)
                # print('obs=', obs, 'new_action=', new_action)

                ## cooperate infomation processing [eric]
                # submit_info_to_env(args, agt_idx, self.mini_policy, agent_name, next_true_value)
                env.step(new_action)
```

3. The Reciever takes action with current policy, according to the observations, which include the public signal, and commit to the environment.
```python
                final_reward = compute_reward(true_value=true_value, pay=reward,
                                              allocation=allocation,
                                              reward_shaping=check_support_reward_shaping(supported_function,
                                                                                          args.reward_shaping),
                                              reward_function_config=build_reward_function_config(args),
                                              user_id=agent_name, budget=self.budget,
                                              args=args, info=info  # compute based on the former budget
                                              )  # reward shaping

                # print(true_value, final_reward)

                # obs record?

                obs = agt.get_last_obs()
                obs = increase_info_to_obs(obs, extra_info_name='allocation', value=allocation)

                # add more obs if not winner only
                if not self.winner_only or allocation:
                    obs = increase_info_to_obs(obs,extra_info_name='true_value',value=true_value)
                    obs = increase_info_to_obs(obs,extra_info_name='payment',value=reward)
                    # print("add true value" , obs['true_value'])



                # update poicy
                agt.update_policy(obs=obs, reward=final_reward, done=truncation)
```

4. Utilitys of the Reciever is observed and each Reciever update her policy with the information of the true state and utility in the last round.

The process is conducted for multiple rounds until the policy is convergent, which indicates the Reciever learned the signal distribution and can make rational decisions. 

---

### Reference
[1] Kamenica E, Gentzkow M. Bayesian persuasion[J]. American Economic Review, 2011, 101(6): 2590-2615.
