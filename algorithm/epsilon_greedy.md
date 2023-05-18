### Introduction 
We first introduce the algorithm of the Epsilon-Greedy [1], which is a common algorithm in reinforcement learning.

![Screen Shot 2023-05-16 at 9.53.50 PM.png](https://intranetproxy.alipay.com/skylark/lark/0/2023/png/158408/1684299252563-1c329cd6-8d1d-4b34-be4e-7a1c7f0b81d8.png#clientId=uf24ee978-0df3-4&from=drop&id=uac541a9a&originHeight=292&originWidth=776&originalType=binary&ratio=1&rotation=0&showTitle=false&size=70575&status=done&style=none&taskId=ub31384ad-4625-4dc4-a9f2-5f7e8e58f4f&title=)

### Implement details 
The algorithm is implemented in  "rl_utils/solver.py".


### Specific examples of Epsilon-Greedy 
We provide an example of how to apply provided algorithm into agent policy decision making.<br />We take "simple_greedy_agent" to become a smart agent as an example, where the detailed definition can be found in the "./agent/custom_agent.py"<br />When the action generator method is called, we apply epsilon greedy algorithm through the "self.algorithem.generate_action"
```python
def generate_action(self, obs):
    encoded_action = self.algorithm.generate_action(obs=self.get_latest_true_value())

    # Decode the encoded action 
    action = encoded_action % (self.algorithm.bidding_range)

    return action
```



---

### Reference

[1] Sutton, R. S., & Barto, A. G. (2018). _Reinforcement learning: An introduction_. MIT press.
