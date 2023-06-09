### Utility function

In reinforcement learning settings, each agent will receive his reward from the environment after he submits his action. In our AI4SS platform, take a auction scenario as an example, each bidders (smart agent) will submit their bid in each step and receive the final payment to compute their reward. However, each bidder may adopt different utility functions based on the final reward ![](https://intranetproxy.alipay.com/skylark/lark/__latex/9630bf6b3095d2c7abc2f420cb10a3f6.svg#card=math&code=r_i%20&id=p7gMy), where his final utility is computed by a utility function ,![](https://intranetproxy.alipay.com/skylark/lark/__latex/c7fc4177f24d4f9e69b46b5e287aefb1.svg#card=math&code=u_i%3DU_i%28r_i%29&id=ufMBu), where ![](https://intranetproxy.alipay.com/skylark/lark/__latex/202a4da434d9cdf3093143fe0622cdcc.svg#card=math&code=U_i&id=yr2Vm)is the agent ![](https://intranetproxy.alipay.com/skylark/lark/__latex/2443fbcfeb7e85e1d62b6f5e4f27207e.svg#card=math&code=i&id=bQPBg)'s utility function.



#### Normal utility function 
In a common setting, bidders regard his reward or the revenue as his utility, which denotes the following utility function <br />![](https://intranetproxy.alipay.com/skylark/lark/__latex/7147c78c70e7a541fefb4b8e41ec4de1.svg#card=math&code=U_i%28r_i%29%20%3DU%28r_i%29%3Dr_i&id=eWAcV)

The hyperparameter in our platform is denoted as below 

| **Args** | **Explanation** |
| --- | --- |
| `arg.reward_shaping   = None ` | reward shaping is None, equalling to a normal utility function.  |

#### 
#### Constant Relative Risk Aversion (CRRA) utility function
In the real world, some bidders will suffer from a bad investment and become unwilling to take a risk. From the economic literature[1,2], considering the Constant Relative Risk Aversion (CRRA) [2] , and its utility function is a common risk aversion utility function, and we implement it in our platform, where the computation is <br />![](https://intranetproxy.alipay.com/skylark/lark/__latex/301446e6a854488ff386ad1d85b0b400.svg#card=math&code=U_%7BCRRA%7D%20%28r_i%29%3D%20r_i%5E%7B%5Cgamma%7D%20&id=z985F) <br />where ![](https://intranetproxy.alipay.com/skylark/lark/__latex/4aa418d6f0b6fbada90489b4374752e5.svg#card=math&code=%5Cgamma&id=uNUu1) is a const that reflects his risk aversion.

The hyperparameter in our platform is denoted as below 

| **Args** | **Explanation** |
| --- | --- |
| `arg.reward_shaping   = 'CRRA" ` | reward shaping is 'CRRA' , equalling to a CRRA function defined in './rl_utils/rewards_shaping.py" |

#### 
#### Constant Absolute Risk Aversion (CARA) utility function
Another risk aversion utility function is the  Constant Absolute Risk-Aversion (CARA) [2], defined as below <br />![](https://intranetproxy.alipay.com/skylark/lark/__latex/72da826a5ace5bcdb00891c61cb80222.svg#card=math&code=U_%7BCARA%7D%28r_i%29%3D1-e%5E%7B-r_i%20z%7D&id=oBwE8)<br />where ![](https://intranetproxy.alipay.com/skylark/lark/__latex/02bab26178a0cd05dae15ad487830237.svg#card=math&code=z&id=ilcXn)is a const that reflects his risk aversion.<br />The hyperparameter in our platform is denoted as below 

| **Args** | **Explanation** |
| --- | --- |
| `arg.reward_shaping   = 'CARA" ` | reward shaping is 'CARA' , equalling to a CRRA function defined in './rl_utils/rewards_shaping.py" |


#### Custom any utility function 

We also provide the method to design any utility function you like, the utility function is defined in the './rl_utils/rewards_shaping.py" with the function "compute_reward ".

```python
def compute_reward(true_value, pay, allocation=0, pay_sign=-1,

                   reward_shaping=False, reward_function_config={}, user_id=None, budget=None, args=None, info=None
                   ):
    if 'other_value' in info:
        other_value = info['other_value']
    else:
        other_value =0



    mode = 'individual'
    if args.inner_cooperate == 1 and other_value is not None:  # can witness other bidder value:

        if args.value_div == 1 and info['cooperate_win']: # cooperate win | other wise allocation =0
            mode ='shared_value_with_pay'
            pay=info['cooperate_pay']

        if args.cooperate_pay_limit == 1 and allocation==1 and pay_sign * pay > true_value:  # win and exceed the pay limit
                allocation=0
                pay=0.1*pay # 10% punish
        true_value += other_value  # only the winner can added on, the rest allocation is 0 thus no influence

    # pay sign ==-1 ---> pay is negative

    reward = None
    payment = pay_sign * pay

    # budget reshape ()
    if budget is not None:
        # in punishment mode:  pay over bid will not receive the item and receive a punishment
        # print('---')
        # print(user_id)
        # print(allocation)
        # print(payment)
        allocation, payment = budget.check_budget_result(user_id=user_id, allocation=allocation, payment=payment)
    #

    # reward computing function
    if mode == 'individual':
        
        # Support both scalar (single item) and vector (multi-item) allocation
        reward = np.dot(true_value, allocation) - payment  # pay_sign * pay 

    elif mode == 'shared_value_with_pay': #already win

        reward = (true_value  - payment)*1.0 / len(args.inner_cooperate_id)   # shared value - shared payment





    # print('reward is ' +str(reward))
    # print('payment is ' + str(payment))
    # print('true_value is ' + str(true_value))
    # print('-----')

    ####### utility compute ####
    if reward_shaping:
        # print(reward_function_config)
        # print(reward)
        if 'function' in reward_function_config:
            if reward_function_config['function'] == 'CRRA':
                reward = CRRA_shaping(reward, a=reward_function_config['a'])
            elif reward_function_config['function'] == 'CARA':
                reward = CARA_shaping(reward, a=reward_function_config['a'])
        else:
            print('not find [function] in config, compute without reward shaping')

    return reward


def CRRA_shaping(reward, a=0.5):
    # u(z)=z^a,a in [0,1]
    # print(reward)
    if reward == 0 or reward is None:
        return 0
    else:
        if reward < 0:
            tmp = ((-1 * reward) ** a) * -1  # deal with nan
        else:
            tmp = reward ** a
        if tmp is np.nan:
            print('exist nan !')

        return tmp


def CARA_shaping(reward, a=0.5):
    # u(z)=1-exp^(-az),a>0
    return 1 - math.exp(-1 * a * reward)

```

As a results, when you determine to build upon a customed utility function, just add your defined utility function in the function config and append your reward shaping function to reshape the reward. Then use the hyperparamter to turn on your utility function with your function name. 

| **Args** | **Explanation** |
| --- | --- |
| `arg.reward_shaping   = 'YOUR_FUNC" ` | reward shaping is the function that you defined , see in  './rl_utils/rewards_shaping.py" |



---

### Reference
[1]Rabin M. Risk aversion and expected-utility theory: A calibration theorem[M]//Handbook of the fundamentals of financial decision making: Part I. 2013: 241-252.<br />[2] [https://web.stanford.edu/class/cme241/lecture_slides/UtilityTheoryForRisk.pdf](https://web.stanford.edu/class/cme241/lecture_slides/UtilityTheoryForRisk.pdf)
