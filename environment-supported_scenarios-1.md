#### Sealed Value-Based Single-Item (Repeated) Auction
Value-based auction is a classic auction where users observe the item valuation before bidding. In this mini-env, we provide interface for the involoved agent with basic agent class, where base agent should realize its profile (including budget, utility or reshaped reward function, valuation distribution) and information observation function along with action generate function (agent policy). More detailed can be found in the Section "Agent". <br />Here we display the auction pipeline with supported scenarios as below. <br />![image.png](https://intranetproxy.alipay.com/skylark/lark/0/2023/png/229273/1684209706671-91917816-e7c9-4266-b449-f71b90ed27a1.png#clientId=uf5c8941e-5149-4&from=paste&height=459&id=u12d5ebe0&originHeight=459&originWidth=680&originalType=binary&ratio=1&rotation=0&showTitle=false&size=236555&status=done&style=none&taskId=u5d9c8b92-e9cf-46cb-9bdf-54cf4e2b4d2&title=&width=680)<br />Fig. Sealed Value-Based Auction Framework
##### Auction format:

- [x] Sealed First Price Auction
- [x] Sealed Second Price Auction
- [x] Sealed Custom Ruled Auction

Noted. Custom Rule means custom design allocation rule & payment rule (See **Custom Mechansim page**)                                                                                                                                              
##### Valuation Distribution format:

- [x] Independent Uniform Distribution (with minimal unit) 
- [x] Independent Gaussian Distribution (with minimal unit) 
- [x] Independent Custom Distribution (with minimal unit) 
- [ ] Dependent Uniform Distribution 
- [ ] Custom Distribution (without minimal unit) 

##### Agent/bidder number:

- [x] Fixed agent number during simulation [ 2 | 3 | 4 | .. | 10 | .. | 100 | .. ]
- [ ] Dynamic agent number during simulation 

##### Other auction settings:

- [x] Reserve price
- [x] Entrance fee
- [ ] <br />

---

#### <br />
