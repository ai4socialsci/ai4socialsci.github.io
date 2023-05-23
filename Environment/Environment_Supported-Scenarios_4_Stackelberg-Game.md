#### Stackelberg Game
In a Stackelberg game, one player (the “leader”) moves first, and all other players (the “followers”) move after him[1]. So the followers should decide their best reponses after seeing which action the leader has choosed, while the leader should deicide his move based on the fact that the followers will move according to their best responses.<br />![stackelberg_game_illustrate.png](https://intranetproxy.alipay.com/skylark/lark/0/2023/png/94556578/1684405022556-9bca3344-8e99-4d86-80d0-0148c78721b8.png#clientId=u90da8256-380d-4&from=drop&height=186&id=uf34c50de&originHeight=788&originWidth=1361&originalType=binary&ratio=2&rotation=0&showTitle=false&size=70943&status=done&style=none&taskId=u150b75f6-5a27-42eb-bad9-bed24a67950&title=&width=321)
#### Notation 
| **Notation** | **Meaning** |
| --- | --- |
| ![](https://intranetproxy.alipay.com/skylark/lark/__latex/ab59bb471329c6102cb4ba6ebedcb2e1.svg#card=math&code=a_l&id=OztGL) | leader action |
| ![](https://intranetproxy.alipay.com/skylark/lark/__latex/ad4444b2fa4d9bb8f5c597dae312be6f.svg#card=math&code=a_f&id=jwFcW) | follower action |
| ![](https://intranetproxy.alipay.com/skylark/lark/__latex/c267807d39aa11ac282a811761574dd7.svg#card=math&code=u_l%28a_l%2Ca_f%29%0A&id=wSF9q) | leader utility |
| ![](https://intranetproxy.alipay.com/skylark/lark/__latex/44685bfca8469d637c48d80837ac3316.svg#card=math&code=u_f%28a_l%2Ca_f%29&id=tfoy1) | follower utility |
| ![](https://intranetproxy.alipay.com/skylark/lark/__latex/08bbd87fd5a99203db9cb13801e401a7.svg#card=math&code=%5Cvarepsilon%28a_l%29&id=lXrTa) | follower best-response function |

#### Stackelberg Equilibrium
To compute a Stackelberg equilibrium, we need to use **backward induction[**1]:** **compute the best reponse function of followers first, which generates the reward-maximizing action for every leader's action, and then decide leader's action, accounting for followers' best responses. This procudure can be observed:<br />![standard_stackelberg.png](https://intranetproxy.alipay.com/skylark/lark/0/2023/png/94556578/1684302964520-17edfac2-d7a8-44b9-b8c3-072a1264a3ce.png#clientId=u71d650ef-35a5-4&from=drop&id=u91a4c308&originHeight=765&originWidth=2997&originalType=binary&ratio=2&rotation=0&showTitle=false&size=128081&status=done&style=none&taskId=u9efa60d2-270c-4273-a0c8-80123b234d3&title=)
#### Stackelberg game Env
While in our environment, we can approximate Stackelberg equilibirum with multi-round agents interaction:<br />we repeatedly train the agents with the following loop:

- At even round, the leader agent learns its best decision while the followers donot update their policy
- At odd round, the followers update their policy given the leader agent's fixed policy

As the training loop proceeds, followers can learn a best-reponse policy for any leader's policy, and the leader can optimize its decision based on followers' best responses, so that all the agents can converge to a Stackelberg equilibrium.<br />In our platform, we formulate mini-env4 (Stackelberg Game) with the following pipeline:<br />![Stackelberg_env.png](https://intranetproxy.alipay.com/skylark/lark/0/2023/png/94556578/1684306479943-c2bff136-3f25-40c9-be1d-69c74258b77d.png#clientId=ub7ce60af-447e-4&from=drop&id=ub4c8cc54&originHeight=1709&originWidth=3529&originalType=binary&ratio=2&rotation=0&showTitle=false&size=407327&status=done&style=none&taskId=u76e04447-c1bc-46a7-91d9-cf723cdea04&title=)<br />One of the greatest advantages of our approximation approach is that we don't need to explicitly calculate the best reponses of followers. By optimizing each agents' policy in an fixed game framework, the best-reponse policy can easily approximated, so that we don't need compilcated math tools to calculate them manually.

---

#### Reference
[1] Johari, Ramesh. "Stackelberg Games." Game Theory with Engineering Applications, Stanford University, 2007.
