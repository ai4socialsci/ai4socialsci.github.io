### Introduction
We first introduce the algorithm of the Multiplicative Weights Update (MWU) Method [1], which is commonly used for decision making, prediction, game theory and algorithm design.<br />![image.png](https://intranetproxy.alipay.com/skylark/lark/0/2023/png/94556578/1688022629593-35a96cab-9c87-4d12-973c-7f32475ec822.png#clientId=uc4c65c03-ff18-4&from=paste&height=508&id=uff77f42c&originHeight=1016&originWidth=2192&originalType=binary&ratio=2&rotation=0&showTitle=false&size=1776145&status=done&style=none&taskId=uca019286-eb51-4747-b5ea-4629a37b521&title=&width=1096)<br />We also implement a variant of MWU algorithm which uses multiplicative function instead of the exponential function above:<br />![](https://intranetproxy.alipay.com/skylark/lark/__latex/c7cd5e58c73b0efa51f839b466e3e641.svg#card=math&code=w_i%5E%7Bt%2B1%7D%20%3D%20w_i%5E%7Bt%7D%20%5Cmax%5C%7B0%2C%281-%5Cepsilon%5Ccdot%5Ctextbf%7BM%7D%28i%2Cj%5Et%29%29%5C%7D.&id=VUh3a)
### Implement details
The algorithm is implemented in "test_mini_env/multiplicative_weights.py", with class ContextualMWU.<br />When deciding how much to bid, MWU algorithm samples bid action according to the weights of each bid, using take_action method as follows:
```python
class ContextualMWU():

    def __init__(self,input_range,bidding_range,overbid,lr,rho,mechanism,player_num,exponential=True):
        self.input_range = input_range
        self.bidding_range = bidding_range
        self.overbid = overbid
        self.lr = lr
        self.rho = rho
        self.mechanism = mechanism
        self.player_num = player_num
        self.exponential = exponential

        if self.overbid:
            self.weights = np.ones([self.input_range, self.bidding_range])
        else:
            # set weights where bid > input with 0
            bids = np.arange(0,self.bidding_range)
            inputs = np.arange(0,self.input_range).reshape(-1,1)
            self.weights = np.where(bids <= inputs, 1.0, 0.0)

    def take_action(self, signal, test=False):
        upper_bound = self.bidding_range if self.overbid else signal + 1
        valid_weights = self.weights[signal][:upper_bound]
        if test:
            return np.argmax(valid_weights)
        return np.random.choice(np.arange(upper_bound), size=1, p=valid_weights / valid_weights.sum())[0]
```
The algorithm updates its weights via update method as follows. It calculates rewards of each bid value with market price, and then update the weights of different bid values according to the exponential or multiplicative update rule:
```python
    def update(self, market_price, signal, agt_value):
        upper_bound = self.bidding_range if self.overbid else signal + 1
        virtual_bid = np.arange(upper_bound)
        if self.mechanism == 'second_price':
            rewards = np.where(virtual_bid > market_price, agt_value-market_price,0.0)
        else:
            rewards = np.where(virtual_bid > market_price, agt_value-virtual_bid,0.0)
        # bid == market price -> estimated reward = (V-p)/N
        if market_price < upper_bound:
            rewards[market_price] = float(agt_value-market_price)/self.player_num

        # update
        if self.exponential:
            # Exponential update:
            # r>0: w_{t+1} = w_{t} * (1+eps)^(r_{t}/rho)
            # r<0: w_{t+1} = w_{t} * (1-eps)^(-r_{t}/rho)
            pos_rate = (1 + self.lr) ** (rewards / self.rho)
            neg_rate = (1 - self.lr) ** (-rewards / self.rho)
            self.weights[agt_value][:upper_bound] *= np.where(rewards > 0, pos_rate, neg_rate)
        else:
            # direct multiplicative update
            # w_{t+1} = w_{t} * (1+eps*r_{t})
            tmp = (1 + self.lr * rewards)
            self.weights[signal][:upper_bound] *= np.where(tmp > 0, tmp, 0)

        # normalize
        self.weights[signal] /= self.weights[signal].sum()
```
### Specific examples of using MWU
Examples of using MWU to solve the equilibrium is provided in "Equilibrium_Explore/Explore_utils.py", which can set the MWU algorithm for bidding agents like:
```python
    elif args.algorithm in ['MWU','mwu']:
        from test_mini_env.multiplicative_weights import \
            get_custom_algorithm_in_mwu,\
            generate_action_mwu,\
            update_policy_mwu, \
            receive_observation_mwu,\
            test_policy_mwu

        def set_mwu_for_agent(custom_agent_name, dynamic_env, custom_algorithm):
            dynamic_env.set_rl_agent_algorithm(algorithm=custom_algorithm,
                                               agent_name=custom_agent_name)
            dynamic_env.set_rl_agent_generate_action(generate_action=generate_action_mwu,
                                                     agent_name=custom_agent_name)
            dynamic_env.set_rl_agent_update_policy(update_policy=update_policy_mwu,
                                                       agent_name=custom_agent_name)
            dynamic_env.set_rl_agent_receive_observation(receive_observation=receive_observation_mwu,
                                                         agent_name=custom_agent_name)
            dynamic_env.set_rl_agent_test_policy(test_policy=test_policy_mwu, agent_name=custom_agent_name)

        for j in range(args.player_num):
            set_mwu_for_agent(custom_agent_name=('player_' + str(j)), dynamic_env=dynamic_env,
                                  custom_algorithm=get_custom_algorithm_in_mwu(args))
```
After that, the bidding agents will act according to generate_action_mwu function described in "test_mini_env/multiplicative_weights.py".
```python
def generate_action_mwu(self,obs):
    if self.args.signal_game:
        signal = obs['public_signal']
        assert self.args.agt_obs_public_signal_dim == 1
        signal = signal[0]
    else:
        signal = self.get_latest_true_value()

    action = self.algorithm.take_action(signal)

    self.record_action(action)

    return action
```

---

### Reference
[1] Arora, S., Hazan, E. and Kale, S., 2012. The Multiplicative Weights Update Method: a Meta Algorithm and Applications. Theory of Computing (2012).
