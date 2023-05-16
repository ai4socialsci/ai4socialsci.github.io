## Start to play classic auction
### Join an auction with custom agents
We recommend users to design custom agents to join different types of auctions (i.e., mini-envs). We build base agent class and users can build upon the base class for the custom agent or directly over-write with part of the func. The base agent class file is located at "./agent/custom_agent.py" <br />![image.png](https://intranetproxy.alipay.com/skylark/lark/0/2023/png/229273/1681283670324-1f4b2a71-60ab-4208-b900-be4f83c2c4ca.png#clientId=u91fad222-39c4-4&from=paste&height=336&id=u7d30555d&originHeight=336&originWidth=696&originalType=binary&ratio=1&rotation=0&showTitle=false&size=138232&status=done&style=none&taskId=uf3caea38-1f87-4a53-b33a-7d27666dba4&title=&width=696)<br />Fig. Agent Function Format 

We present an example to illustrate how to design a custom agent to join an auction. Try our example on Mini-env1 (sealed single-item value based auction) with the following command:
```shell
#run the value based auction 
python easy_start_agent_policy.py

```

The agent function definition is located in "./test_player_func.py" 
```python
from agent.custom_agent import *
from rl_utils.solver import *

def get_custom_agent_name():
    return 'player_3'
        
def get_custom_rl_agent(args):
    return custom_agent(get_custom_agent_name(), highest_value=args.valuation_range - 1)

def get_custom_algorithm(args):
    return EpsilonGreedy_auction(bandit_n=args.valuation_range * args.bidding_range,
                                 bidding_range=args.bidding_range, eps=0.01,
                                 start_point=int(args.exploration_epoch),
                                 # random.random()),
                                 overbid=args.overbid, step_floor=int(args.step_floor),
                                 signal=args.public_signal)  # 假设bid 也从分布中取
    
def generate_action(self, obs):
    # print('self', self)
    # print('obs', obs)
    encoded_action = self.algorithm.generate_action(obs=self.get_latest_true_value())

    # Decode the encoded action 
    action = encoded_action % (self.algorithm.bidding_range)

    self.record_action(action)
    return action
    # return 1

def update_policy(self, obs, reward, done):
    last_action = self.get_latest_action()
    if done:
        true_value = self.get_latest_true_value()
    else:
        true_value = self.get_last_round_true_value()
    encoded_action = last_action + true_value * (self.algorithm.bidding_range)
    self.algorithm.update_policy(encoded_action, reward=reward)

    self.record_reward(reward)
    return

def receive_observation(self, args, budget, agent_name, agt_idx, 
                        extra_info, observation, public_signal):
    obs = dict()
    obs['observation'] = observation['observation']
    if budget is not None:
        budget.generate_budeget(user_id=agent_name)
        obs['budget'] = budget.get_budget(user_id=agent_name)  # observe the new budget

    if args.communication == 1 and agt_idx in args.cm_id and extra_info is not None:
        obs['extra_info'] = extra_info[agent_name]['others_value']

    if args.public_signal:
        obs['public_signal'] = public_signal
    return obs

    

```

The mini-env set the agent by the following codes in "./easy_start_agent_policy.py" 
```python
 # adjust agent policy (include three metioned method)：
        dynamic_env.set_rl_agent(rl_agent=custom_rl_agent,
                                 agent_name=custom_agent_name) #adjust agent type
                # can provide more agent for user design based on the basic agent
                # example: add fixed agent (we provided example)
                # example: add greedy agent ( now different agents are designed due to different format obs)

        ## only adjust agent algorithm
        dynamic_env.set_rl_agent_algorithm(algorithm=custom_algorithm, 
                                           agent_name=custom_agent_name) #search agt.generate_action(obs)
        dynamic_env.set_rl_agent_generate_action(generate_action=test_player.generate_action, 
                                                 agent_name=custom_agent_name)
        dynamic_env.set_rl_agent_update_policy(update_policy=test_player.update_policy, 
                                               agent_name=custom_agent_name) # add function search agt.update_policy | user can user other infos in basic agents(see in  normal_agent.py )
        dynamic_env.set_rl_agent_receive_observation(receive_observation=test_player.receive_observation, 
                                                     agent_name=custom_agent_name) #  add function init_reward_list | search receive_observation()
```

The agent policies will be plotted after exploration. From theoretical results, bidders from the same iid distrbution will bid truthfully, and the final equilibrium is plotted.

![image.png](https://intranetproxy.alipay.com/skylark/lark/0/2023/png/229273/1681284338742-b1a52fe5-99d8-4e78-971f-4013beaefb43.png#clientId=u05dcf58c-1745-4&from=paste&height=240&id=ubff632f7&originHeight=240&originWidth=320&originalType=binary&ratio=1&rotation=0&showTitle=false&size=14749&status=done&style=none&taskId=u10bc8dcf-99ec-49e1-8626-c56a38ee945&title=&width=320)<br />Fig. Agent Policy Example 

---

