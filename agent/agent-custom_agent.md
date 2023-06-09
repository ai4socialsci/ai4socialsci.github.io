## Custom agent from inherent
One straightforward way to design a custom agent is to inherit from provided basic agent class, such as "Base agent" or "Signal agent". We provide two detailed example to illustrate how to build upon a customed agent from inherent. 

### Inherit example 1 : Fixed bid agent
We provide a agent class with fixed bid policy or the oracle bididng policy with "fixed_agent", which is located in "./agent/normal_agent.py" as well.
```python
class fixed_agent(base_agent):
    """
    Fixed learner places its true value as its bid
    """

```

#### Independent valuation generator
In this example, we illustrate a simple fixed bid agent with independent valuation generator. In other word, in every step of mini-env auction, bidder will generate a item value and submit bid according to the value realization. The detailed generator function is defined in the "generate_true_value " as below, with uniform distritbuion as well as gaussian distribution. More valuation distrbutions can be easily extended in the following function. 
```python
    def generate_true_value(self,method='uniform'):
        """
        Generate the true value of the auction item for an agent
        """ 
        if self.item_num == 1: 
            if method =='uniform':
                true_value= random.randint(self.lowest_value,self.highest_value)  #[0-H]
            elif method =='gaussian': # Wrong implementation
                true_value = int(np.random.uniform(self.lowest_value, self.highest_value+1))
            #else:
            #    true_value
                # print(true_value)# remained for other generation method
        else: 
            if method =='uniform':
                true_value= [random.randint(self.lowest_value,self.highest_value) for i in range(self.item_num) ]  #[0-H]
            elif method =='gaussian': # Wrong implementation
                true_value = [int(np.random.uniform(self.lowest_value, self.highest_value+1)) for i in range(self.item_num)] 

        self.record_true_value(true_value)
        return
```

#### Fixed bid policy 
As the fixed bid agent only bid with his true value, thus he requires no algorithm. We implement his action generation method as below,
```python
    def generate_action(self,observation):
        """
        Fixed learner always bid its true value
        """
        action = self.get_latest_true_value()
        self.record_action(action)


        return action
```

---

### Inherit example 2 : Greedy agent
We also provide a agent class with greedy bid policy with "fixed_agent", which is located in "./agent/normal_agent.py" as well. This example illustrates the rational bidders who generate action to argmax his expected reward. In order to learn from historical experience, all the bidding history along with allocation history will be utilized. 
```python
class greedy_agent(base_agent):
    """
    Greedy agent generates its action based on certain greedy 'self.algorithm'. 
    """ 
```


#### Independent valuation generator
In this example, we illustrate a simple greedy bid agent with independent valuation generator. In other word, in every step of mini-env auction, bidder will generate a item value and submit bid according to the value realization. The detailed generator function is defined in the "generate_true_value " as below, with uniform distritbuion as well as gaussian distribution. More valuation distrbutions can be easily extended in the following function. 
```python
    def generate_true_value(self,method='uniform'):
        """
        Generate the true value of the auction item for an agent
        """ 
        if self.item_num == 1: 
            if method =='uniform':
                true_value= random.randint(self.lowest_value,self.highest_value)  #[0-H]
            elif method =='gaussian': # Wrong implementation
                true_value = int(np.random.uniform(self.lowest_value, self.highest_value+1))
            #else:
            #    true_value
                # print(true_value)# remained for other generation method
        else: 
            if method =='uniform':
                true_value= [random.randint(self.lowest_value,self.highest_value) for i in range(self.item_num) ]  #[0-H]
            elif method =='gaussian': # Wrong implementation
                true_value = [int(np.random.uniform(self.lowest_value, self.highest_value+1)) for i in range(self.item_num)] 

        self.record_true_value(true_value)
        return
```

#### Greedy bid policy 
The greedy bid agent will bid based on his algorithm,  and we also implement the situation where the required bidding item number is larger than 1.
```python
    def generate_action(self,observation):
        
        if self.item_num  == 1: 
            # Generate the action given its observed true value of the item
            encoded_action = self.algorithm.generate_action(obs=self.get_latest_true_value())

            # Decode the encoded action 
            action = encoded_action % (self.algorithm.bidding_range)
        else: 
            action = []
            for i, true_value in enumerate(self.get_latest_true_value()): 
                # Generate the action given its observed true value of the item
                encoded_action = self.algorithm[i].generate_action(obs=true_value)
                # Decode the encoded action 
                action.append(encoded_action % (self.algorithm[i].bidding_range))

        self.record_action(action)  # to guoyang: you left out the record_action ->multi-item =list | single item =int

        return action
```


#### Update policy 
As the agent will learn from the historical results, we also rewrite the update bidding policy, the detailed update algorithm is determined by the algorithm, where we use "self.algorithm.update_policy(encoded_action,reward=reward)".
```python
    def update_policy(self, state, reward,done=False):

        last_action = self.get_latest_action()
        if done:
            true_value = self.get_latest_true_value()
        else:
            true_value =self.get_last_round_true_value()
        if self.item_num == 1: 
            encoded_action = last_action + true_value * (self.algorithm.bidding_range)
            self.algorithm.update_policy(encoded_action,reward=reward)
        else: 
            for i, true_val in enumerate(true_value): 
                #ori ver : encoded_action = last_action + true_val * (self.algorithm[i].bidding_range)
                #eric added: last action should be record
                encoded_action = last_action[i] + true_val * (self.algorithm[i].bidding_range)

                self.algorithm[i].update_policy(encoded_action,reward=reward)

        self.record_reward(reward)

        return
```

---


## Custom agent from initial
Another method to custom a brand new agent is to rebuild a base agent class, which provide more power function and record more required information, such as observed from specific scenarios. We provide a example on the "./agent/custom_agent.py".

### Initial example 1 : simple_greedy_agent 
As denoted in the base agent, we can also design more complex policy along with required informaton collection in "receive observation". To meet the requirement with more information, we rewrite a new class named "custom_agent " with more information record. <br />![image.png](https://intranetproxy.alipay.com/skylark/lark/0/2023/png/229273/1681364815729-ebc894e8-d5cd-45b2-ace0-e5afa267e02f.png#clientId=u05dcf58c-1745-4&from=paste&height=223&id=u0a6e9244&originHeight=223&originWidth=503&originalType=binary&ratio=1&rotation=0&showTitle=false&size=67793&status=done&style=none&taskId=u1f64a082-ecd5-4c4f-b3dd-f9cc5d35b1f&title=&width=503)<br />Fig1. Base agent class.

Then, we can formulate the simple greedy agent based on the custom agent class with default algorithm.
```python
class simple_greedy_agent(custom_agent):
    def __init__(self, args=None, agent_name='', lowest_value=0, highest_value=10):
        super(simple_greedy_agent, self).__init__(agent_name, lowest_value, highest_value)

        self.args = args

        self.set_algorithm(
            EpsilonGreedy_auction(bandit_n=(highest_value + 1) * self.args.bidding_range,
                                  bidding_range=self.args.bidding_range, eps=0.01,
                                  start_point=int(self.args.exploration_epoch),
                                  # random.random()),
                                  overbid=self.args.overbid, step_floor=int(self.args.step_floor),
                                  signal=self.args.public_signal
                                  )  # 假设bid 也从分布中取
        )
```

#### Observation rebuild
We provide the detailed example to rebuild the observation by agent himself, where all the information in the observation is record from a specifc environment. 
```python
def receive_observation(self, args, budget, agent_name, agt_idx, 
                            extra_info, observation, public_signal,
                            true_value_list, action_history, reward_history, allocation_history):
        obs = dict()
        obs['observation'] = observation['observation']
        if budget is not None:
            budget.generate_budeget(user_id=agent_name)
            obs['budget'] = budget.get_budget(user_id=agent_name)  # observe the new budget

        if args.communication == 1 and agt_idx in args.cm_id and extra_info is not None:
            obs['extra_info'] = extra_info[agent_name]['others_value']

        if args.public_signal:
            obs['public_signal'] = public_signal

        obs['true_value_list'] = true_value_list
        obs['action_history'] = action_history
        obs['reward_history'] = reward_history
        obs['allocation_history'] = allocation_history
        
        return obs
```

#### Greedy bid policy 
The greedy bid agent will bid based on his algorithm with the observations in the current state.

```python
    def generate_action(self, obs):
        encoded_action = self.algorithm.generate_action(obs=self.get_latest_true_value())

        # Decode the encoded action 
        action = encoded_action % (self.algorithm.bidding_range)

        return action
```
