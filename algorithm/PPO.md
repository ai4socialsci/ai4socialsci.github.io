### Introduction 
We first introduce the algorithm of the PPO [1] , which is a common algorithm of the Multi-arm bandit solver in reinforcement learning.

![Screen Shot 2023-05-16 at 11.42.51 PM.png](https://intranetproxy.alipay.com/skylark/lark/0/2023/png/158408/1684305808109-b1c385cc-3e7a-43a8-9293-8b1640b52f90.png#clientId=u9d627278-ea4f-4&from=drop&id=u447c3e4c&originHeight=349&originWidth=776&originalType=binary&ratio=1&rotation=0&showTitle=false&size=84067&status=done&style=none&taskId=ud3e7a682-2e19-4b9c-92a5-c733b59907c&title=)

### Implement details 
The algorithm is implemented in the "./test_mini_env/torch_ppo_example.py" where  


### Specific examples of PPO
We provide a example of how to apply provided algorithm into agent policy decision making.<br />We take "simple_greedy_agent" to become a smart agent as an example, where the detailed definition can be found in the "./agent/custom_agent.py"<br />When the action generator method is called, we apply epsilon greedy algorithm through the "self.algorithem.generate_action"
```python
def generate_action(self, obs):
    encoded_action = self.algorithm.generate_action(obs=self.get_latest_true_value())

    # Decode the encoded action 
    action = encoded_action % (self.algorithm.bidding_range)

    return action
```



---

### Reference
[1] Schulman, J., Wolski, F., Dhariwal, P., Radford, A., & Klimov, O. (2017). Proximal policy optimization algorithms. _arXiv preprint arXiv:1707.06347_.
