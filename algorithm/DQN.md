### Introduction 
We first introduce the algorithm of the Deep Q-learning (DQN) [1] , which is a common algorithm of the Q-learning tabular solver in reinforcement learning.

![Screen Shot 2023-05-16 at 10.19.07 PM.png](https://intranetproxy.alipay.com/skylark/lark/0/2023/png/158408/1684300756184-7336c46a-b04f-499e-ae1c-38c04b390d07.png#clientId=udd235df1-64e7-4&from=drop&id=u7264423e&originHeight=348&originWidth=784&originalType=binary&ratio=1&rotation=0&showTitle=false&size=96433&status=done&style=none&taskId=u14a66bc8-5475-4200-b7fd-6d24d5567ce&title=)
### Implement details 
Our platform supports PyTorch/TensorFlow based policy.  A summary figure is shown below.<br />![image.png](https://intranetproxy.alipay.com/skylark/lark/0/2023/png/229273/1684239330894-7cca2f6a-ba5c-4004-aff2-08b758bd8095.png#clientId=u9ef636ca-9c8c-4&from=paste&height=231&id=u94794572&originHeight=231&originWidth=452&originalType=binary&ratio=1&rotation=0&showTitle=false&size=78781&status=done&style=none&taskId=u53d7a68f-8752-4555-9e38-7162def48c0&title=&width=452)<br />Fig1. Basic agent class where the bidding policy is one of the 4 core function. 

![image.png](https://intranetproxy.alipay.com/skylark/lark/0/2023/png/229273/1681202956445-676f129d-3cbe-45ca-be24-4ef3330ecd83.png#clientId=u7f55ab39-3a24-4&from=paste&height=322&id=u050bd815&originHeight=322&originWidth=546&originalType=binary&ratio=1&rotation=0&showTitle=false&size=108755&status=done&style=none&taskId=u3ab57f48-0dba-4446-965c-3f5304e2b15&title=&width=546)<br />Fig2. Pytorch Policy Example 

As denoted in the Pytorch Interface, we provide the example of build a custom DQN model and training during policy updating in the "/test_mini_env/pytorch_algo_example.py" .


We first build a DQN model and define the way from model output to a determined bidding action as below.
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




### Specific examples of DQN 
We provide a example of how to apply provided algorithm into agent policy decision making.<br />We take "simple_greedy_agent" to become a smart agent as an example, where the detailed definition can be found in the "./test_mini_env/pytorch_algo_example.py"

The example network framework is ploted as below,<br />![image.png](https://intranetproxy.alipay.com/skylark/lark/0/2023/png/229273/1684240356402-1931801b-1356-46a1-bf23-dc5c4d4b0a2e.png#clientId=u89d5cec3-2dbe-4&from=paste&height=268&id=uf85ce65d&originHeight=268&originWidth=181&originalType=binary&ratio=1&rotation=0&showTitle=false&size=42916&status=done&style=none&taskId=u34263dae-eaf0-48fe-b6de-21f9f6a9059&title=&width=181)<br />Fig3.  Example DQN network. 

Then we apply Second price auction with 4 agents using Adam +MSE loss, and detailed hyperparameters can be found in the  "./test_mini_env/test_signal_game.py" and set the mode in the "easy_signal_setting " to "pytorch" . The final result is plotted as below. 

| **Args** | **Explanation** |
| --- | --- |
| `mode  = pytorch  ` | the test signal game mode  |

#### 

![image.png](https://intranetproxy.alipay.com/skylark/lark/0/2023/png/229273/1684240475817-d7fc40d1-15b5-4fb9-8da3-9a2cbec1835a.png#clientId=u89d5cec3-2dbe-4&from=paste&height=290&id=u54b115cb&originHeight=290&originWidth=390&originalType=binary&ratio=1&rotation=0&showTitle=false&size=118099&status=done&style=none&taskId=ua42ce253-57dd-4b7d-a0c2-ec389730091&title=&width=390)<br />Fig4.  DQN Policy after 400,000 iters on Second Price.


#### Improvement with CFR loss 
However, classical MSE loss function may failed in the model updating due to limited information, as only winner can receive a non-zero reward. As a result, we introduce CFR loss[] , which is widely used in the Poke filed. The theoretical formate of CFR is 

Here we show a example that all the bidders adopt the same DQN method and all bidder acquire information about the payment and the item value after 400,000 auctions on Sealed Second Price auction. 

![image.png](https://intranetproxy.alipay.com/skylark/lark/0/2023/png/229273/1681379951093-cd3673cf-fca2-4745-be55-5c647543a3d7.png#clientId=u93eed3ba-e5d2-4&from=paste&id=g4cNM&originHeight=223&originWidth=297&originalType=binary&ratio=1&rotation=0&showTitle=false&size=28669&status=done&style=none&taskId=u3a30a1e7-8c10-4afe-94c9-24ed8b5ef85&title=)<br />Fig5. DQN+CFR Policy after 400,000 iters on Second Price.

---

### Reference
[1] Mnih, V., Kavukcuoglu, K., Silver, D., Rusu, A. A., Veness, J., Bellemare, M. G., ... & Hassabis, D. (2015). Human-level control through deep reinforcement learning. _nature_, _518_(7540), 529-533.
