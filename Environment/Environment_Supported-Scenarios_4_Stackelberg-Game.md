#### Stackelberg Game
In a Stackelberg game, one player (the “leader”) moves first, and all other players (the “followers”) move after him[1]. So the followers should decide their best reponses after seeing which action the leader has choosed, while the leader should deicide his move based on the fact that the followers will move according to their best responses.
#### Stackelberg Equilibrium
To compute a Stackelberg equilibrium, we need to use **backward induction[**1]:** **compute the best reponse function of followers first, which calculates the reward-maximizing action for every leader's action, and then decide leader's action, accounting for followers' best responses. This procudure can be observed:<br />![standard_stackelberg.png](https://intranetproxy.alipay.com/skylark/lark/0/2023/png/94556578/1684302964520-17edfac2-d7a8-44b9-b8c3-072a1264a3ce.png#clientId=u71d650ef-35a5-4&from=drop&id=u91a4c308&originHeight=765&originWidth=2997&originalType=binary&ratio=2&rotation=0&showTitle=false&size=128081&status=done&style=none&taskId=u9efa60d2-270c-4273-a0c8-80123b234d3&title=)
#### Stackelberg game Env
While in our environment, we can approximate Stackelberg equilibirum with multi-round agents interaction:<br />we repeatedly train the agents with the following loop:

- At even round, the leader agent learns its best decision while the followers donot update their policy
- At odd round, the followers update their policy given the leader agent's fixed policy

As the training loop proceeds, followers can learn a best-reponse policy for any leader's policy, and the leader can optimize its decision based on followers' best responses, so that all the agents can converge to a Stackelberg equilibrium.<br />In our platform, we formulate mini-env4 (Stackelberg Game) with the following pipeline:<br />![Stackelberg_env.png](https://intranetproxy.alipay.com/skylark/lark/0/2023/png/94556578/1684306479943-c2bff136-3f25-40c9-be1d-69c74258b77d.png#clientId=ub7ce60af-447e-4&from=drop&id=ub4c8cc54&originHeight=1709&originWidth=3529&originalType=binary&ratio=2&rotation=0&showTitle=false&size=407327&status=done&style=none&taskId=u76e04447-c1bc-46a7-91d9-cf723cdea04&title=)<br />One of the greatest advantages of our approximation approach is that we don't need to explicitly calculate the best reponses of followers. By optimizing each agents' policy in an fixed game framework, the best-reponse policy can easily approximated, so that we don't need compilcated math tools to calculate them manually.

---

#### Reference
[1] Johari, Ramesh. "Stackelberg Games." Game Theory with Engineering Applications, Stanford University, 2007.
