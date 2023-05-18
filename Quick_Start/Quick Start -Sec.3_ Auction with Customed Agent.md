## Start to play a classic auction with customed agents
### Join an auction with provided agent function interface
We recommend users to design customized agents to join different types of auctions (i.e., mini-envs). We build base agent class and users can build upon the base class for the custom agent or directly over-write part of the func. The base agent class file is located at "./agent/custom_agent.py" <br />[Liuyi: "directly over-write part of the func." directly over-write the agent class?]

![image.png](https://intranetproxy.alipay.com/skylark/lark/0/2023/png/229273/1681283670324-1f4b2a71-60ab-4208-b900-be4f83c2c4ca.png#clientId=u91fad222-39c4-4&from=paste&height=336&id=u7d30555d&originHeight=336&originWidth=696&originalType=binary&ratio=1&rotation=0&showTitle=false&size=138232&status=done&style=none&taskId=uf3caea38-1f87-4a53-b33a-7d27666dba4&title=&width=696)<br />Fig. Agent Function Format 

[Liuyi: 1. in the figure:  customed -> customized; "用户视角" ; not very clear-costomized agent?]

We present an example to illustrate how to design a custom agent to join an auction. Try our example on Mini-env1 (sealed single-item value based auction) with the following command:
```shell
#run the value based auction 
python easy_start_agent_policy.py

```

The agent function definition is located in "./test_player_func.py" <br />[Liuyi: I am originally confused about this part T.T: Why those funcs are separately put here? how to use them? See func get_custom_agent_name(), it doesn't look like a normal func. After looking the original code, I suggest the following: <br />The additional methods of agent class is defined in "./test_player_func.py" , and they are binded to the agent class (mini-env class) through its set_rl_agent_algorithm, set_rl_agent_generate_action, <br />       set_rl_agent_update_policy, set_rl_agent_receive_observation methods.<br />] 

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

#### Example Results

We illustrate a replacement of provided customed agent in a detailed example : Sealed single item auction with codes in "./easy_start_agent_policy.py".<br />As a results, the agent policies will be plotted after exploration. From theoretical results, bidders from the same iid distrbution will bid truthfully, and the final equilibrium is plotted.

![image.png](https://intranetproxy.alipay.com/skylark/lark/0/2023/png/229273/1681284338742-b1a52fe5-99d8-4e78-971f-4013beaefb43.png#clientId=u05dcf58c-1745-4&from=paste&height=240&id=ubff632f7&originHeight=240&originWidth=320&originalType=binary&ratio=1&rotation=0&showTitle=false&size=14749&status=done&style=none&taskId=u10bc8dcf-99ec-49e1-8626-c56a38ee945&title=&width=320)<br />Fig1. Customized Agent Policy Example 

---

### Join an auction with custom designed agents
We recommend to build a custom agent based on our provided base agent where you only require to re-write part of the function. For different auctions, we also provide a default agent with a simple bidding policy (Epsilon greedy). You can also design more complex policy along with required informaton collection in "receive observation". <br />[Liuyi: last sentence is not clear. "receive observation" where?]<br />![image.png](https://intranetproxy.alipay.com/skylark/lark/0/2023/png/229273/1681364815729-ebc894e8-d5cd-45b2-ace0-e5afa267e02f.png#clientId=u05dcf58c-1745-4&from=paste&height=223&id=u0a6e9244&originHeight=223&originWidth=503&originalType=binary&ratio=1&rotation=0&showTitle=false&size=67793&status=done&style=none&taskId=u1f64a082-ecd5-4c4f-b3dd-f9cc5d35b1f&title=&width=503)<br />Fig. Base agent class.
### 
#### Extend provided bidding policy or other function

In our agent decision making, each agent will generate agent through the function "generate action". The main function in policy design is the method "update policy" and "generate action".<br />[Liuyi: 1. "each agent will generate agent" is it a typo? generate agent? 2. not very clear: where are those functions in the code? or they are implemented by user if customized?]

We provide many detailed examples in our codes [Liuyi: detailed location?].

##### Rule-based Policy
We provide a simple rule based policy in "/test_mini_env/test_signal_game_example_function.py"<br />Both the input and output format are standarded and you only require to complete the custom policy within each function. Note that the input obs are defined in the function "Receive Observation" where all the observation are provided and other stastic information can be added if custom.

We provide a example where action is generated through an EpsilonGreedy algorithm, which is defined through the self.algorithm. The algorithm is defined through the agent class initialization.
```python
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


### assign the algorthm 
#dynamic_env.set_rl_agent_algorithm(algorithm=custom_algorithm,
 #                                          agent_name=custom_agent_name) #search agt.generate_action(obs)
```

##### Pytorch Policy
As denoted in the Pytorch Interface, we provide the example of build a custom DQN model and training during policy updating in the "/test_mini_env/pytorch_algo_example.py" .<br />[Liuyi: not sure whether we should put so many code in the quick start part?]<br />We first build a DQN model and define the way from model output to a determined bidding action as below.
```python
class example_Net(nn.Module):
    def __init__(self, input_range, output_range,device='cpu'):
        super().__init__()

        self.input_dim=input_range
        self.output_dim=output_range
        self.device=device


        self.model = nn.Sequential(
            nn.Linear(np.prod(input_range), 128), nn.ReLU(inplace=True),
            nn.Linear(128, 64), nn.ReLU(inplace=True),
            nn.Linear(64, 128), nn.ReLU(inplace=True),
            nn.Linear(128, np.prod(output_range)),
        )

    def obs_encode(self,obs):
        # input N*1 dim obs denotes as batch -> N*M tensor
        # eg.one-hot encode

        if not isinstance(obs, torch.Tensor):
            obs = torch.tensor(obs)

        one_hot_emb = torch.nn.functional.one_hot(obs,num_classes=self.input_dim) #[0, N-1]

        return one_hot_emb.float().to(self.device)


    def set_seed(args,seed=0):
        # Set seed for result reproducibility
        random.seed(seed)
        os.environ['PYTHONHASHSEED'] = str(seed)
        np.random.seed(seed)
        torch.manual_seed(seed)
        torch.backends.cudnn.deterministic = True
        torch.backends.cudnn.benchmark = False



    def forward(self, obs, state=None, info={}):
        #print(obs)


        encoded_obs = self.obs_encode(obs)
        #batch = encoded_obs.shape[0]


        # compute q value
        logits = self.model(encoded_obs)

        # argmax q value
        max_value,act = logits.max(dim=-1)

        # batchsize *1

        return logits,max_value,act



class pytorch_algorithm_class(object):
        def __init__(self,args,max_bidding_value,seed):

            device = torch.device("cuda:0" if torch.cuda.is_available() else "cpu")

            self.model = example_Net( input_range=args.public_signal_range, output_range=max_bidding_value,device=device)
            self.model.set_seed(seed=seed) # if set seed

            self.loss_fn =nn.MSELoss()#nn.CrossEntropyLoss() #nn.MSELoss()

            self.loss_fn2 = nn.CrossEntropyLoss() # nn.CrossEntropyLoss() #nn.MSELoss()

            self.optimizer=torch.optim.Adam(self.model.parameters(), lr=args.lr)

            self.model.to(device)

            self.max_bidding_value=max_bidding_value
```

Then we also use the interface of "algorithm" from the base agent and rewrite the defination of algorithm. (We will show later about how to assign the function towards a specific agent).
```python
def get_custom_algorithm_in_pytorch(args,algorithm='DQN',seed=0):

    # build a algorithm for the rllib

    return pytorch_algorithm_class(args,max_bidding_value=args.bidding_range,seed=seed)
```

Then, the generate bidding action function is re-writed as below:

```python
def generate_action_pytorch(self, obs):
    # print('self', self)
    # print('obs', obs)
    # encoded_action = self.algorithm.generate_action(obs=self.get_latest_true_value())

    signal = obs['public_signal']

    # action, _, _ = self.algorithm.compute_single_action(
    #     encoded_input_emb(signal, action_space_num=self.args.bidding_range)
    # )
    if self.record_data_num<self.args.exploration_epoch:

        action = random.randint(0,self.args.bidding_range-1)

    else:
        #print('start !')
        self.algorithm.model.eval()
        with torch.no_grad():
            logits, max_value, tensor_action = self.algorithm.model(signal)  # return as tensor without grad

        action = int(tensor_action.item())


    # Decode the encoded action
    #action = encoded_action % (self.algorithm.bidding_range)

    self.record_action(action)

    return action
```

The core question may raise that how to train the model and build up a datalodaer. Here we should an simple example about how to train a model. A more complex dataloader can be designed by users.

```python
def update_policy_pytorch(self, obs, reward, done):


    if obs['allocation']==1 or ('true_value' in obs) : #skip the first round or knows the true value
        #only record allocation =1 or during exploration epoch


        self.record_signal(obs['public_signal'])


        ### try to optimize best convegence
        if 'true_value' in obs :
            #knows true value
            self.record_true_value(obs['true_value'])

            if obs['allocation']==0:
                #and not allocate
                action=self.action_history[-1]
                reward = obs['true_value']-action #assume if win

        self.record_reward(reward)

        self.record_data_num+=1

        batchsize=self.args.batchsize
        new_loss=self.args.cfr_loss

        #self.args.exploration_epoch> self.record_data_num  #try to use two type loss [loss1:reward no-regret loss | loss 2: reconstruct loss]

        if self.record_data_num % batchsize==0 : #self.algrithm.update_frequency:
            input_batch = self.signal_history[-batchsize:]
            y_batch = self.reward_history[-batchsize:]

            # rebuild into pytorch version

            if not isinstance(input_batch, torch.Tensor):
                input_batch=torch.tensor(input_batch)

            if not isinstance(y_batch, torch.Tensor):
                y_batch = torch.tensor(y_batch)

                #            #for better train
                true_value = torch.tensor( self.true_value_list[-batchsize:])+1 #start from 1
                true_value=true_value.float().cuda().reshape([batchsize, 1])


                #y_batch = y_batch / true_value #use utility/value rate


            y_batch=y_batch.float().cuda().reshape([batchsize,1]) #/ self.algorithm.max_bidding_value

            self.algorithm.model.train()
            self.algorithm.model.cuda()
            with torch.enable_grad():
                logsit,max_value,pred = self.algorithm.model(input_batch) # input with grad

            #expected_reward = logsit[pred]


           # loss = self.algorithm.loss_fn(max_value, y_batch) #compute  reward - expected reward loss




            # algorithm Backpropagation
            self.algorithm.optimizer.zero_grad() #optimizer init
            #loss.backward() #compute loss

            if (new_loss):
                #consider the reconstruct loss
                # pay = true_value - reward
                pay = true_value - y_batch #*true_value

                second_flag=False
                if self.args.mechanism=='second_price':
                    second_flag=True
                K = 1
                logsit_size = self.args.bidding_range

                virtual_bid = torch.tensor([j for j in range(logsit_size)]).reshape(1, K, -1).cuda()
                win_bid_idx = virtual_bid - pay.unsqueeze(-1)
                if second_flag:
                    val_diff = (true_value - pay).unsqueeze(-1).repeat(1, 1, win_bid_idx.shape[-1])
                    #tmp = true_value.unsqueeze(-1).repeat(1, 1, win_bid_idx.shape[-1]) - virtual_bid
                else:
                    val_diff = true_value.unsqueeze(-1).repeat(1, 1, win_bid_idx.shape[-1]) - virtual_bid

                # print(val_diff.shape)
                # print(tmp.shape)
                idea_logsit1 = torch.zeros(logsit.size()).cuda()
                idea_logsit1[win_bid_idx >= 0] = val_diff[win_bid_idx >= 0]





                loss2=self.algorithm.loss_fn(logsit, idea_logsit1)
                loss2.backward()

                if self.record_data_num % (batchsize*1000) ==0:
                    print('agent(' + str(self.agent_name) + ') epoch ' + str(
                        self.record_data_num / batchsize) + ' --> the training loss(avg mse)  with batch size ' + str(
                        batchsize) + ' is ' + str(loss2.item()))
            else:
                loss = self.algorithm.loss_fn(max_value, y_batch)  # compute  reward - expected reward loss
                loss.backward()  # compute loss
                if self.record_data_num % (batchsize * 1000) == 0:
                    print('agent(' + str(self.agent_name) + ') epoch ' + str(
                        self.record_data_num / batchsize) + ' --> the training loss(mse)  with batch size ' + str(
                        batchsize) + ' is ' + str(loss.item()))

            self.algorithm.optimizer.step() #step


    return
```

Here we show a example that all the bidders adopt the same DQN method and all bidder acquire information about the payment and the item value after 400,000 auctions on Sealed Second Price auction. 

![image.png](https://intranetproxy.alipay.com/skylark/lark/0/2023/png/229273/1681379951093-cd3673cf-fca2-4745-be55-5c647543a3d7.png#clientId=u93eed3ba-e5d2-4&from=paste&id=u3ceea3c8&originHeight=223&originWidth=297&originalType=binary&ratio=1&rotation=0&showTitle=false&size=28669&status=done&style=none&taskId=u3a30a1e7-8c10-4afe-94c9-24ed8b5ef85&title=)<br />Fig. DQN Policy after 400,000 iters on Second Price.
