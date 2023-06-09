
### Introduction
We recommend to build a custom agent based on our provided base agent where you only require to re-write part of the function. For different auctions, we also provide a default agent with a simple bidding policy (Epsilon greedy). You can also design more complex policy along with required informaton collection in "receive observation". <br />![image.png](https://intranetproxy.alipay.com/skylark/lark/0/2023/png/229273/1681364815729-ebc894e8-d5cd-45b2-ace0-e5afa267e02f.png#clientId=u05dcf58c-1745-4&from=paste&height=223&id=u0a6e9244&originHeight=223&originWidth=503&originalType=binary&ratio=1&rotation=0&showTitle=false&size=67793&status=done&style=none&taskId=u1f64a082-ecd5-4c4f-b3dd-f9cc5d35b1f&title=&width=503)<br />Fig1. Base agent class.

### Basic prosperity
We introduce the basic prosperity in the basic agent, where the class name is  "base agent" , and the detail codes are located in "./agent/normal_agent.py". 

| **Function** | **Explanation** |
| --- | --- |
| `generate_action(observation)    ` | Generate action based on the observation and other infos stored in the class. |
| `generate_true_value ()    ` | Will become available if each agent independent generate item true value rather than the platform reveal the true value or other signals.  |
| `update_policy (state,reward)    ` | Update action generate policy based on the state and reward if required.  |
| `get_last_action ()    ` | Return the last submit action or the bidding price. |
| `get_latest_action ()` | Return the current submit action or the bidding price before the auction finished in this step.  |
| `get_latest_true_value ()    ` | Return the current true value in this step. |
| `get_latest_action ()` | Return the current submit action or the bidding price before the auction finished in this step.  |
| `get_action_history  ()    ` | Get the historical action list.  |
| `get_reward_history  ()` | Get the historical reward list  |
| `get_allocation_history  ()    ` | Get the historical allocation result list  |
| `get_averaged_reward  (epoch,allocation_only)` | Get the average reward on the latest epoch, and allocation_only=True means only compute the reward at win. |
| `set_algorithm (algorithm ,item_num)` | Set the agent algorithm, allow to adopt differnent algorithms with different item number. |
| `record_reward  (reward)` | Record the reward. |
| `record_action  (action)    ` | Record the latest aciton. |
| `record_allocation   (allocation)    ` | Record the allocation result. |

#### 



---

### Inherit example 1 : Fixed bid agent
We also provide a agent class with fixed bid policy or the oracle bididng policy with "fixed_agent", which is located in "./agent/normal_agent.py" as well.
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
