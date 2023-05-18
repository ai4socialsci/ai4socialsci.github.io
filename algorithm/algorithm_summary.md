### Summary 
Based on the implement of the agent class (see Fig.1) , we split action policy (or the bidding policy) into the algorithm class where each agent bidding policy can be easily export or inherit. <br />![image.png](https://intranetproxy.alipay.com/skylark/lark/0/2023/png/229273/1684239330894-7cca2f6a-ba5c-4004-aff2-08b758bd8095.png#clientId=u9ef636ca-9c8c-4&from=paste&height=231&id=u94794572&originHeight=231&originWidth=452&originalType=binary&ratio=1&rotation=0&showTitle=false&size=78781&status=done&style=none&taskId=u53d7a68f-8752-4555-9e38-7162def48c0&title=&width=452)<br />Fig1. Basic agent class where the bidding policy is one of the 4 core function. 

We implement several popular agent policy including multi-arm bandit algorithm as well as deep learning algorithm. Some implements are build upon the Pytorch or Rllib. <br />Noted that other algorithm such as the function to determine whether to continuely join the auction or the function that computes the revealed information can also be implemented by the algorithm when given the same input format. 

The detailed implements can be found in the following list:

- [x] Oracle policy 
- [x] Epsilon-Greedy [1]
- [x] DQN [2]
- [ ] PPO [3]
- [ ] ...


---

### Reference
[1] Sutton, R. S., & Barto, A. G. (2018). _Reinforcement learning: An introduction_. MIT press.<br />[2] Mnih, V., Kavukcuoglu, K., Silver, D., Rusu, A. A., Veness, J., Bellemare, M. G., ... & Hassabis, D. (2015). Human-level control through deep reinforcement learning. _nature_, _518_(7540), 529-533.<br />[3] Schulman, J., Wolski, F., Dhariwal, P., Radford, A., & Klimov, O. (2017). Proximal policy optimization algorithms. _arXiv preprint arXiv:1707.06347_.
