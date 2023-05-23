# Summary

## Platform
### Platform Description
This platform is a multi-agent reinforcement learning platform designed for AI4SS（AI for social science). <br />Social science is the study of how people interact with one another. The branches of social science include anthropology, economics, political science, psychology, and sociology [1]. In order to figure out how human react to the complex envirionment and finally reach the ideal equilibriums, we design a platform named AI4SS with multi-agent reinforcement learning algorithem to simulate possible situations in the Social Science. We use smart agents to simulate human behaviors in real world and each agent is supported to be easily customed by users. Meanwhile, we also support social planner to adjust the environment rules during the games such as allocation rule and payment rule and information disclusre rule,etc. 

In summary, this platform implements multiple simuation enviroments in the "**Auction**" scenarios[2], which are frequently appeared in our lifes, and other issues such as "Stackelberg Game[3]", "Bayesian Persuasion[4]" are also included. <br />![image.png](https://intranetproxy.alipay.com/skylark/lark/0/2023/png/229273/1684142276658-4ae78817-1063-4ebe-bb79-c5d327619d9c.png#clientId=uea805165-abd5-4&from=paste&height=509&id=u0d135e9c&originHeight=509&originWidth=620&originalType=binary&ratio=1&rotation=0&showTitle=false&size=237919&status=done&style=none&taskId=uede5a168-e7f7-467c-a11f-cf96e8c1eba&title=&width=620)<br />Figure 1. **An overview to the platform.** Our framework design is motivated by real-world observations, which consists of multiple implemented components.

Below we provide an overview of our platform:<br />[Liuyi: 

1. These four parts may seem to be parallel, but in fact, they have a hierarchical relationship. I'm not sure whether using four bullets is the best way to express
2. The four parts are not well aligned with figure 1 or figure 1 is not well aligned with the four component]
- **Platform Design**:  The following are the key components in out framework. 
   - _**Core execution environment **_: The core execution environment is consist of multi round mini-envs and each mini-env is applied to compute their best response or equlibrium with given mechanism. In the core execution environment, social planner determine the mechanism and thus the mini-env is determined. Then, agents performs their best response in each round. When the whole environment is run only 1 round, the core execution environment can be regard to compute equlibrium with a fixed social planner rule . 
   - _**Mini-env **_: Mini-env is also a  multi-agent envirionment which is implemented with PettingZoo [Liuyi: add the link/ref of PettingZoo] format. In mini-env, the mechanism and the information policy is determined and will run multiply steps to compute agent best response. 
   - **Social Planner (Mechanism)**: Allocation rule and payment rule are the key rules in the mechanism. The information rule or the signal distribution are also included in this section. 
   - **Smart Agent **: We apply multi-agent reinforcement learning during the whole framework.  [Liuyi: It seems not a description?] 

![image.png](https://intranetproxy.alipay.com/skylark/lark/0/2023/png/229273/1684143239526-9be321a5-ca77-4c8d-b206-22ac11620a48.png#clientId=ud18959f2-2cfd-4&from=paste&height=492&id=u72fe45ca&originHeight=492&originWidth=898&originalType=binary&ratio=1&rotation=0&showTitle=false&size=283610&status=done&style=none&taskId=ud0867bba-377b-41ce-baa5-27bfe5a8549&title=&width=898)<br />Figure 2. **An overview to the environment design.**

- **Environment Design：**We provide multiple auction environment implements including single item/multi item auction, sealed auction and ascending auction, Stackelberg Game environment as well as other signal game environment with auction rules. To introduce the effects of mechanism or the social planner (platform), we segement the execution environment into multiple blocks, which we called mini-env. [Liuyi: Not sure whether it can use "segement".1. Is mini-env like plugin format?  2. From context, it seems that the mulple multi-envs all exits simutanuesly? I guess actualy, during each whole env running round, only one is selected? If so, maybe: "we provide multiple types of mini-envs, including ....."3. mini-env is also mentioned in the previous platform design part]During each round of the core execution environment, the social platform can adjust the mechanism or directly switch mini-env blocks. Then users will react to the new envs based on the former learned policy. When simply use 1 block, the equlibrium can be computed based on the provided enviroments. We implement several customized mini-env blocks as below, which can be easily extended by users. 
   - _**Sealed value based auction** _: Value-based auction is a classic auction where users observe the item valuation before bidding.
   - _**Sealed signal based auction **_: Signal-based auction is the auction where each user/bidder observes the signal rather than the value of the item. The signal comprises a public signal and a private signal, and the value of the item is the sum of the public value and the private value.
   - **Open auction** : Open auction is the auction that each bidder publicly report their bid and sequentially submit their bid, such as ascending price auction. 
   - **Stackelberg Game** :  Leader and follower sequentially behave and compute their best response based on their information and the opposite expected action.

- **Social Planner Design：**We allow social planner to adjust environment rules during the environment running process. In auction scenarios, the mechanism design and the information design are the crucial components. We provide two detailed examples for the mechanism design and information design to illustrate how social planner can influence the social welfare. 
   - _**Mechanism design **_: Design a better mechanism (payment rule and allocation) than the standard first or second price mechanism to maximize social planner objective such as social welfare.
   - _**Bayesian persuasion **_: Design a platform informed signal to the agent to change agents' posterior probability.

![image.png](https://intranetproxy.alipay.com/skylark/lark/0/2023/png/229273/1684142662770-1cea2ac1-b4d8-4972-8903-b06f06b4aa5a.png#clientId=ud18959f2-2cfd-4&from=paste&height=283&id=uf2756f8d&originHeight=283&originWidth=487&originalType=binary&ratio=1&rotation=0&showTitle=false&size=109212&status=done&style=none&taskId=ub4daa737-2d30-46cc-a253-77acf65add9&title=&width=487)<br />Figure 3. **An overview to the social planner functionality.**

- **Agent Design：**We provide multiple default agent types inherited from a basic agent class. The agent will observe the environment, behave according to the observation and optimize their behavior policy. All the three bevhaiors are implemented with three functions with standard input and output format.  Other funcationalities are aslo supported such as refreshing private signal/value or recording any important information or adjust utility function. See more details in the "Agent Design".[Liuyi: add the link/ref] 

![image.png](https://intranetproxy.alipay.com/skylark/lark/0/2023/png/229273/1684143386943-b6ce39aa-ed3f-4a2d-b024-535b45c52ce3.png#clientId=ud18959f2-2cfd-4&from=paste&height=259&id=u83245106&originHeight=259&originWidth=488&originalType=binary&ratio=1&rotation=0&showTitle=false&size=92616&status=done&style=none&taskId=ub24e6d9f-8e21-4948-9511-c71b9de0731&title=&width=488)<br />Figure 4. **An overview to the agent class functionality.**

- **Agent Policy Design** :  We implement multiple agent policy with standard input and output such as oracle policy, greedy policy and deep learning based policy. You can easily implement any policy to guide smart agent's behavior in the complex situation with provided input and output format. 
   - _**Oracle** _:  we allow agent to perform any action followed by the customed oracle policy as in real world people cannot always behave rationally. 
   - _**Greedy policy** _: When we assume agent is rational, one straightforward policy is to perform a greedy policy where agent choose to perform the action that argmax his expected reward. As a results,  we implement Epsilon-greedy method as an example. 
   - **Deep learning policy**: We also implement deep learning based policy such as DQN and PPO algorithm. [Liuyi: ref/link of DQN & PPO]

- **Framework Dependency/Support**: In designing and training smart agents, our platform supports several popular deep learning (DL) libraries (such as PyTorch and TensorFlow) and reinforcement learning libraries (such as RLlib).

- **Platform Contribution**: 
   - _**Simulate real-world environment **_: With provided AI4SS environment, we can simulate social planner's policy or mechanism influences and figure out possible real world phenomenon without real-world cost. 
   - _**Explore assymetric or complex equilibriums** _:  We can relax hypothesis in the theoretical social science field and apply our platform to explore assymetric or complex equilibriums. 
   - ...

Need to refine ![image.png](https://intranetproxy.alipay.com/skylark/lark/0/2023/png/229273/1681201371341-223e75a3-8bb6-418c-87ae-1caf89b55bd1.png#clientId=u7f55ab39-3a24-4&from=paste&height=606&id=jH2pr&originHeight=606&originWidth=1088&originalType=binary&ratio=1&rotation=0&showTitle=false&size=134235&status=done&style=none&taskId=u4ef1d5d2-5f78-4e66-ad94-80d8dcb315d&title=&width=1088)

Figure x. **An overview to the platform.** Our framework design is motivated by real-world observations. On the agent side, agents can choose to join or leave the auction game in each round; they adopt various policies to make their bids. They can communicate and exchange their information/observation. Across multiple epochs of learning, the agents first perform random exploration and then update their policy. On the environment side, an auction game is consisted of an allocation rule and a payment rule. We support multiple advaced features at both the agent side and the environment side. (Need to re-design)



---

# Reference
[1] Social Science Definition：[https://www.investopedia.com/terms/s/social-science.asp](https://www.investopedia.com/terms/s/social-science.asp)<br />[2] Klemperer P. Auction theory: A guide to the literature[J]. Journal of economic surveys, 1999, 13(3): 227-286.<br />[3]Stackelberg Game Wiki: [https://en.wikipedia.org/wiki/Stackelberg_competition](https://en.wikipedia.org/wiki/Stackelberg_competition)<br />[4] Kamenica E, Gentzkow M. Bayesian persuasion[J]. American Economic Review, 2011, 101(6): 2590-2615.
