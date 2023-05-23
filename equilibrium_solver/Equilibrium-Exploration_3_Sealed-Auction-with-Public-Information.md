## Theoritcal equilibrium with public value
### Basic setting
We follow the basic setting in our auction simulated environment from [1], which consists of the following components with sealed second price or first price mechanism (Vickery Auction[2]). We first introduce the pure public value setting, which is a special case of the common value auction[1]. <br /> 

- Independent bidders  ![](https://intranetproxy.alipay.com/skylark/lark/__latex/4787ef6775bbb255dcfeea378956293f.svg#card=math&code=i%3D1%2C2..N&id=pECGU)
- Mechanism ![](https://intranetproxy.alipay.com/skylark/lark/__latex/4b8b9b0fabafa7af21a4ead38095519f.svg#card=math&code=M%3D%3Cg%2Cp%3E&id=yTpsX)
- Signal ![](https://intranetproxy.alipay.com/skylark/lark/__latex/976c8910cfdedd8f22080f224b66e9b4.svg#card=math&code=S_1%2CS_2%2C..S_n%20&id=ITVaj) with joint density ![](https://intranetproxy.alipay.com/skylark/lark/__latex/3a8a245b476dfeedc06568c1e163468f.svg#card=math&code=f%28.%29&id=ivkSj) are exchangeable and affilated
   - Exchangeable : ![](https://intranetproxy.alipay.com/skylark/lark/__latex/3fbf90ceda0db6d4798b510fd9c7ae11.svg#card=math&code=s%27&id=UyLYR) is the permutation of ![](https://intranetproxy.alipay.com/skylark/lark/__latex/79ce3c7a71877c2ff01695e38ade43ca.svg#card=math&code=s&id=jPdqR) and ![](https://intranetproxy.alipay.com/skylark/lark/__latex/7d868fb9e00bcb286ebe017bafd20ff0.svg#card=math&code=f%28s%29%3Df%28s%27%29&id=pTpDx)
   - affilated: 
- Pure public value : all bidders have the same value, given by some random variable ![](https://intranetproxy.alipay.com/skylark/lark/__latex/9f493997c33913987175caf4a4849955.svg#card=math&code=V&id=JsLrF), where signal ![](https://intranetproxy.alipay.com/skylark/lark/__latex/976c8910cfdedd8f22080f224b66e9b4.svg#card=math&code=S_1%2CS_2%2C..S_n%20&id=c27yI) are correlated with ![](data:image/svg+xml;utf8,%3Csvg%20xmlns%3Axlink%3D%22http%3A%2F%2Fwww.w3.org%2F1999%2Fxlink%22%20width%3D%2214.062ex%22%20height%3D%222.676ex%22%20style%3D%22vertical-align%3A%20-0.671ex%3B%22%20viewBox%3D%220%20-863.1%206054.6%201152.1%22%20role%3D%22img%22%20focusable%3D%22false%22%20xmlns%3D%22http%3A%2F%2Fwww.w3.org%2F2000%2Fsvg%22%20aria-labelledby%3D%22MathJax-SVG-1-Title%22%3E%0A%3Ctitle%20id%3D%22MathJax-SVG-1-Title%22%3E%24S_1%2CS_2%2C..S_n%24%3C%2Ftitle%3E%0A%3Cdefs%20aria-hidden%3D%22true%22%3E%0A%3Cpath%20stroke-width%3D%221%22%20id%3D%22E1-MJMAIN-24%22%20d%3D%22M162%20187Q162%20164%20146%20149T109%20133H103V130Q108%20115%20115%20105Q122%2092%20131%2082T150%2064T170%2052T190%2044T206%2040T220%2037L227%2036V313Q190%20320%20162%20335Q116%20358%2086%20404T55%20508Q55%20567%2085%20614T165%20685Q186%20696%20225%20704H227V750H273V704L286%20703Q369%20690%20413%20631Q441%20588%20444%20531Q444%20514%20443%20509Q439%20490%20425%20479T391%20468Q368%20468%20353%20483T337%20522Q337%20546%20353%20560T390%20575L394%20576V578Q386%20599%20372%20614T342%20637T314%20649T288%20656L273%20658V408L288%20405Q329%20394%20355%20376Q396%20348%20420%20300T444%20199Q444%20130%20408%2076T313%201Q286%20-9%20276%20-9H273V-56H227V-10H221Q202%20-6%20193%20-4T155%2011T108%2041T74%2094T55%20176V182Q55%20227%2095%20238Q103%20240%20108%20240Q129%20240%20145%20226T162%20187ZM225%20657Q219%20657%20204%20651T169%20632T135%20594T121%20538Q121%20512%20131%20491T156%20457T187%20435T213%20423T227%20420V539Q227%20657%20225%20657ZM378%20169Q378%20230%20339%20265T274%20301Q273%20301%20273%20169V37Q324%2050%20351%2087T378%20169Z%22%3E%3C%2Fpath%3E%0A%3Cpath%20stroke-width%3D%221%22%20id%3D%22E1-MJMATHI-53%22%20d%3D%22M308%2024Q367%2024%20416%2076T466%20197Q466%20260%20414%20284Q308%20311%20278%20321T236%20341Q176%20383%20176%20462Q176%20523%20208%20573T273%20648Q302%20673%20343%20688T407%20704H418H425Q521%20704%20564%20640Q565%20640%20577%20653T603%20682T623%20704Q624%20704%20627%20704T632%20705Q645%20705%20645%20698T617%20577T585%20459T569%20456Q549%20456%20549%20465Q549%20471%20550%20475Q550%20478%20551%20494T553%20520Q553%20554%20544%20579T526%20616T501%20641Q465%20662%20419%20662Q362%20662%20313%20616T263%20510Q263%20480%20278%20458T319%20427Q323%20425%20389%20408T456%20390Q490%20379%20522%20342T554%20242Q554%20216%20546%20186Q541%20164%20528%20137T492%2078T426%2018T332%20-20Q320%20-22%20298%20-22Q199%20-22%20144%2033L134%2044L106%2013Q83%20-14%2078%20-18T65%20-22Q52%20-22%2052%20-14Q52%20-11%20110%20221Q112%20227%20130%20227H143Q149%20221%20149%20216Q149%20214%20148%20207T144%20186T142%20153Q144%20114%20160%2087T203%2047T255%2029T308%2024Z%22%3E%3C%2Fpath%3E%0A%3Cpath%20stroke-width%3D%221%22%20id%3D%22E1-MJMAIN-31%22%20d%3D%22M213%20578L200%20573Q186%20568%20160%20563T102%20556H83V602H102Q149%20604%20189%20617T245%20641T273%20663Q275%20666%20285%20666Q294%20666%20302%20660V361L303%2061Q310%2054%20315%2052T339%2048T401%2046H427V0H416Q395%203%20257%203Q121%203%20100%200H88V46H114Q136%2046%20152%2046T177%2047T193%2050T201%2052T207%2057T213%2061V578Z%22%3E%3C%2Fpath%3E%0A%3Cpath%20stroke-width%3D%221%22%20id%3D%22E1-MJMAIN-2C%22%20d%3D%22M78%2035T78%2060T94%20103T137%20121Q165%20121%20187%2096T210%208Q210%20-27%20201%20-60T180%20-117T154%20-158T130%20-185T117%20-194Q113%20-194%20104%20-185T95%20-172Q95%20-168%20106%20-156T131%20-126T157%20-76T173%20-3V9L172%208Q170%207%20167%206T161%203T152%201T140%200Q113%200%2096%2017Z%22%3E%3C%2Fpath%3E%0A%3Cpath%20stroke-width%3D%221%22%20id%3D%22E1-MJMAIN-32%22%20d%3D%22M109%20429Q82%20429%2066%20447T50%20491Q50%20562%20103%20614T235%20666Q326%20666%20387%20610T449%20465Q449%20422%20429%20383T381%20315T301%20241Q265%20210%20201%20149L142%2093L218%2092Q375%2092%20385%2097Q392%2099%20409%20186V189H449V186Q448%20183%20436%2095T421%203V0H50V19V31Q50%2038%2056%2046T86%2081Q115%20113%20136%20137Q145%20147%20170%20174T204%20211T233%20244T261%20278T284%20308T305%20340T320%20369T333%20401T340%20431T343%20464Q343%20527%20309%20573T212%20619Q179%20619%20154%20602T119%20569T109%20550Q109%20549%20114%20549Q132%20549%20151%20535T170%20489Q170%20464%20154%20447T109%20429Z%22%3E%3C%2Fpath%3E%0A%3Cpath%20stroke-width%3D%221%22%20id%3D%22E1-MJMAIN-2E%22%20d%3D%22M78%2060Q78%2084%2095%20102T138%20120Q162%20120%20180%20104T199%2061Q199%2036%20182%2018T139%200T96%2017T78%2060Z%22%3E%3C%2Fpath%3E%0A%3Cpath%20stroke-width%3D%221%22%20id%3D%22E1-MJMATHI-6E%22%20d%3D%22M21%20287Q22%20293%2024%20303T36%20341T56%20388T89%20425T135%20442Q171%20442%20195%20424T225%20390T231%20369Q231%20367%20232%20367L243%20378Q304%20442%20382%20442Q436%20442%20469%20415T503%20336T465%20179T427%2052Q427%2026%20444%2026Q450%2026%20453%2027Q482%2032%20505%2065T540%20145Q542%20153%20560%20153Q580%20153%20580%20145Q580%20144%20576%20130Q568%20101%20554%2073T508%2017T439%20-10Q392%20-10%20371%2017T350%2073Q350%2092%20386%20193T423%20345Q423%20404%20379%20404H374Q288%20404%20229%20303L222%20291L189%20157Q156%2026%20151%2016Q138%20-11%20108%20-11Q95%20-11%2087%20-5T76%207T74%2017Q74%2030%20112%20180T152%20343Q153%20348%20153%20366Q153%20405%20129%20405Q91%20405%2066%20305Q60%20285%2060%20284Q58%20278%2041%20278H27Q21%20284%2021%20287Z%22%3E%3C%2Fpath%3E%0A%3C%2Fdefs%3E%0A%3Cg%20stroke%3D%22currentColor%22%20fill%3D%22currentColor%22%20stroke-width%3D%220%22%20transform%3D%22matrix(1%200%200%20-1%200%200)%22%20aria-hidden%3D%22true%22%3E%0A%20%3Cuse%20xlink%3Ahref%3D%22%23E1-MJMAIN-24%22%20x%3D%220%22%20y%3D%220%22%3E%3C%2Fuse%3E%0A%3Cg%20transform%3D%22translate(500%2C0)%22%3E%0A%20%3Cuse%20xlink%3Ahref%3D%22%23E1-MJMATHI-53%22%20x%3D%220%22%20y%3D%220%22%3E%3C%2Fuse%3E%0A%20%3Cuse%20transform%3D%22scale(0.707)%22%20xlink%3Ahref%3D%22%23E1-MJMAIN-31%22%20x%3D%22867%22%20y%3D%22-213%22%3E%3C%2Fuse%3E%0A%3C%2Fg%3E%0A%20%3Cuse%20xlink%3Ahref%3D%22%23E1-MJMAIN-2C%22%20x%3D%221567%22%20y%3D%220%22%3E%3C%2Fuse%3E%0A%3Cg%20transform%3D%22translate(2013%2C0)%22%3E%0A%20%3Cuse%20xlink%3Ahref%3D%22%23E1-MJMATHI-53%22%20x%3D%220%22%20y%3D%220%22%3E%3C%2Fuse%3E%0A%20%3Cuse%20transform%3D%22scale(0.707)%22%20xlink%3Ahref%3D%22%23E1-MJMAIN-32%22%20x%3D%22867%22%20y%3D%22-213%22%3E%3C%2Fuse%3E%0A%3C%2Fg%3E%0A%20%3Cuse%20xlink%3Ahref%3D%22%23E1-MJMAIN-2C%22%20x%3D%223080%22%20y%3D%220%22%3E%3C%2Fuse%3E%0A%20%3Cuse%20xlink%3Ahref%3D%22%23E1-MJMAIN-2E%22%20x%3D%223525%22%20y%3D%220%22%3E%3C%2Fuse%3E%0A%20%3Cuse%20xlink%3Ahref%3D%22%23E1-MJMAIN-2E%22%20x%3D%223970%22%20y%3D%220%22%3E%3C%2Fuse%3E%0A%3Cg%20transform%3D%22translate(4415%2C0)%22%3E%0A%20%3Cuse%20xlink%3Ahref%3D%22%23E1-MJMATHI-53%22%20x%3D%220%22%20y%3D%220%22%3E%3C%2Fuse%3E%0A%20%3Cuse%20transform%3D%22scale(0.707)%22%20xlink%3Ahref%3D%22%23E1-MJMATHI-6E%22%20x%3D%22867%22%20y%3D%22-213%22%3E%3C%2Fuse%3E%0A%3C%2Fg%3E%0A%20%3Cuse%20xlink%3Ahref%3D%22%23E1-MJMAIN-24%22%20x%3D%225554%22%20y%3D%220%22%3E%3C%2Fuse%3E%0A%3C%2Fg%3E%0A%3C%2Fsvg%3E#card=math&code=V&id=vrWAC), but independent conditional on it. (For example, ![](https://intranetproxy.alipay.com/skylark/lark/__latex/126a723d47a390ebdd581d8f4a9aba24.svg#card=math&code=S_i%3DV%2B%5Cepsilon_i&id=WJ81j), ![](https://intranetproxy.alipay.com/skylark/lark/__latex/236a44c1587bf12ed6508c80d0977499.svg#card=math&code=%5Cepsilon_i&id=gC9tG) are independent)
- Bidder ![](https://intranetproxy.alipay.com/skylark/lark/__latex/2443fbcfeb7e85e1d62b6f5e4f27207e.svg#card=math&code=i&id=S1WKr)'s value is ![](https://intranetproxy.alipay.com/skylark/lark/__latex/9875110461cef0663df7945376c99be5.svg#card=math&code=v_i%28s_i%2Cs_%7B-i%7D%29%20%3D%20E%5BV%20%7C%20s_1%2Cs_2..s_n%5D&id=iBDFL) 


### Wallet-game auction[3]:
Definition:<br />Two bidders bid in a SPSB auction for the value of a wallet with two cells, where each of them privately observes the content of only one of the cells.  ![](https://intranetproxy.alipay.com/skylark/lark/__latex/ca3cc99a77092b4a870a2ed346911759.svg#card=math&code=s_1&id=Z8dK3) and ![](https://intranetproxy.alipay.com/skylark/lark/__latex/1ec65b375dec2920064daf545cb0476a.svg#card=math&code=s_2&id=MjAJ2)represent the privately observed signals by the first and second bidder respectively. The value of the wallet is determined by all the signal ![](https://intranetproxy.alipay.com/skylark/lark/__latex/d4eaf3a7f812a02b5e7372230ab10d92.svg#card=math&code=V_1%3DV_2%3Ds_1%2Bs_2&id=LnqtB).<br /> As a result, bidding twice of their observed signal is the unique symmetric equilibrium ![](https://intranetproxy.alipay.com/skylark/lark/__latex/c62b9394172c2a1ad752a5326ecf2fc9.svg#card=math&code=b_i%28s_i%29%3D2s_i&id=MpM1Y)


## Experiment equilibrium validation  

### Setting 1. Symmetric agent in SP 

#### Experiment setting 
##### exp1-1 :  2 bidders in sealed SP auction 
We first validate the results in the **Wallet-game  **with 2 bidders with discrete valuation sampled from Uniform distribution ![](https://intranetproxy.alipay.com/skylark/lark/__latex/19fa58097401a8abe4bb347163258d84.svg#card=math&code=F_i%3DU%5B0%2C10%5D&id=al8tN) where we apply smart agent to simulate the bidder's behaviors and explore his optmial bidding policy. 

##### exp1-2 : ablation study multiple bidders (related to common value setting)
We then increase bidder number to explore possible bidding policy. We test agent learned policy when the bidder number is 2, 3, 4, 5. All bidders' valuation is sampled from uniform distribution ![](https://intranetproxy.alipay.com/skylark/lark/__latex/19fa58097401a8abe4bb347163258d84.svg#card=math&code=F_i%3DU%5B0%2C10%5D&id=OuQrn).

##### exp1-3 : value range
We then enlarge the **Wallet-game  **with 2 bidders with discrete valuation sampled from Uniform distribution ![](https://intranetproxy.alipay.com/skylark/lark/__latex/415f0367b8abeb083d29aebbc052fc47.svg#card=math&code=F_i%3DU%5B0%2C100%5D&id=UTIZ1) where we apply smart agent to simulate the bidder's behaviors and explore his optmial bidding policy. 

#### runnable script
The runnable script is recorded in the "./Equilibrium_Explore/script/run_cv_exp3.sh"

```shell

# exp1-1
python Explore_3_CV.py --mode 'exp1_1' --mechanism 'second_price'

# exp 1-2
python Explore_3_CV.py --mode 'exp1_2' --mechanism 'second_price' --player_num 3
python Explore_3_CV.py --mode 'exp1_2' --mechanism 'second_price' --player_num 4
python Explore_3_CV.py --mode 'exp1_2' --mechanism 'second_price' --player_num 5

#exp 1-3

python Explore_3_CV.py --mode 'exp1_3' --mechanism 'second_price' --player_num 2
python Explore_3_CV.py --mode 'exp1_3' --mechanism 'second_price' --player_num 3

```
#### Theoritical results
According to Wallet-game auction[3], the theoretical results of exp1-1 is to bid twice of the observed signal in ![](data:image/svg+xml;utf8,%3Csvg%20xmlns%3Axlink%3D%22http%3A%2F%2Fwww.w3.org%2F1999%2Fxlink%22%20width%3D%222.759ex%22%20height%3D%222.509ex%22%20style%3D%22vertical-align%3A%20-0.338ex%3B%22%20viewBox%3D%220%20-934.9%201188.1%201080.4%22%20role%3D%22img%22%20focusable%3D%22false%22%20xmlns%3D%22http%3A%2F%2Fwww.w3.org%2F2000%2Fsvg%22%20aria-labelledby%3D%22MathJax-SVG-1-Title%22%3E%0A%3Ctitle%20id%3D%22MathJax-SVG-1-Title%22%3EEquation%3C%2Ftitle%3E%0A%3Cdefs%20aria-hidden%3D%22true%22%3E%0A%3Cpath%20stroke-width%3D%221%22%20id%3D%22E1-MJMAIN-32%22%20d%3D%22M109%20429Q82%20429%2066%20447T50%20491Q50%20562%20103%20614T235%20666Q326%20666%20387%20610T449%20465Q449%20422%20429%20383T381%20315T301%20241Q265%20210%20201%20149L142%2093L218%2092Q375%2092%20385%2097Q392%2099%20409%20186V189H449V186Q448%20183%20436%2095T421%203V0H50V19V31Q50%2038%2056%2046T86%2081Q115%20113%20136%20137Q145%20147%20170%20174T204%20211T233%20244T261%20278T284%20308T305%20340T320%20369T333%20401T340%20431T343%20464Q343%20527%20309%20573T212%20619Q179%20619%20154%20602T119%20569T109%20550Q109%20549%20114%20549Q132%20549%20151%20535T170%20489Q170%20464%20154%20447T109%20429Z%22%3E%3C%2Fpath%3E%0A%3Cpath%20stroke-width%3D%221%22%20id%3D%22E1-MJMATHI-73%22%20d%3D%22M131%20289Q131%20321%20147%20354T203%20415T300%20442Q362%20442%20390%20415T419%20355Q419%20323%20402%20308T364%20292Q351%20292%20340%20300T328%20326Q328%20342%20337%20354T354%20372T367%20378Q368%20378%20368%20379Q368%20382%20361%20388T336%20399T297%20405Q249%20405%20227%20379T204%20326Q204%20301%20223%20291T278%20274T330%20259Q396%20230%20396%20163Q396%20135%20385%20107T352%2051T289%207T195%20-10Q118%20-10%2086%2019T53%2087Q53%20126%2074%20143T118%20160Q133%20160%20146%20151T160%20120Q160%2094%20142%2076T111%2058Q109%2057%20108%2057T107%2055Q108%2052%20115%2047T146%2034T201%2027Q237%2027%20263%2038T301%2066T318%2097T323%20122Q323%20150%20302%20164T254%20181T195%20196T148%20231Q131%20256%20131%20289Z%22%3E%3C%2Fpath%3E%0A%3Cpath%20stroke-width%3D%221%22%20id%3D%22E1-MJMATHI-74%22%20d%3D%22M26%20385Q19%20392%2019%20395Q19%20399%2022%20411T27%20425Q29%20430%2036%20430T87%20431H140L159%20511Q162%20522%20166%20540T173%20566T179%20586T187%20603T197%20615T211%20624T229%20626Q247%20625%20254%20615T261%20596Q261%20589%20252%20549T232%20470L222%20433Q222%20431%20272%20431H323Q330%20424%20330%20420Q330%20398%20317%20385H210L174%20240Q135%2080%20135%2068Q135%2026%20162%2026Q197%2026%20230%2060T283%20144Q285%20150%20288%20151T303%20153H307Q322%20153%20322%20145Q322%20142%20319%20133Q314%20117%20301%2095T267%2048T216%206T155%20-11Q125%20-11%2098%204T59%2056Q57%2064%2057%2083V101L92%20241Q127%20382%20128%20383Q128%20385%2077%20385H26Z%22%3E%3C%2Fpath%3E%0A%3C%2Fdefs%3E%0A%3Cg%20stroke%3D%22currentColor%22%20fill%3D%22currentColor%22%20stroke-width%3D%220%22%20transform%3D%22matrix(1%200%200%20-1%200%200)%22%20aria-hidden%3D%22true%22%3E%0A%20%3Cuse%20xlink%3Ahref%3D%22%23E1-MJMAIN-32%22%20x%3D%220%22%20y%3D%220%22%3E%3C%2Fuse%3E%0A%3Cg%20transform%3D%22translate(500%2C412)%22%3E%0A%20%3Cuse%20transform%3D%22scale(0.707)%22%20xlink%3Ahref%3D%22%23E1-MJMATHI-73%22%20x%3D%220%22%20y%3D%220%22%3E%3C%2Fuse%3E%0A%20%3Cuse%20transform%3D%22scale(0.707)%22%20xlink%3Ahref%3D%22%23E1-MJMATHI-74%22%20x%3D%22469%22%20y%3D%220%22%3E%3C%2Fuse%3E%0A%3C%2Fg%3E%0A%3C%2Fg%3E%0A%3C%2Fsvg%3E#card=math&code=2%5E%7Bst%7D&id=mdUCG)(second price auction) , denoted as  ![](https://intranetproxy.alipay.com/skylark/lark/__latex/c62b9394172c2a1ad752a5326ecf2fc9.svg#card=math&code=b_i%28s_i%29%3D2s_i&id=YYLuS)

When the bidder number increase, the bidder will bid ![](https://intranetproxy.alipay.com/skylark/lark/__latex/d7fd436ce87ea1b1462cd962976e7b66.svg#card=math&code=b_i%28s_i%29%20%3D%20v%28s_i%2Cs_i%29%3DE%5Bv_i%7CX_i%3Ds_i%2CX_%7B-i%7D%3Ds_i%5D&id=ujTg1) where ![](https://intranetproxy.alipay.com/skylark/lark/__latex/339f5c9d413f060b60cfaa17a502a059.svg#card=math&code=X_i%20&id=ScpvK)denotes his signal and the ![](https://intranetproxy.alipay.com/skylark/lark/__latex/7ca2ff984e95d4a9e44fe7498e281020.svg#card=math&code=X_i&id=i6JQT)denotes the opposite maximal signal. 

#### Experiment results
We display our exploration with MARL in AI4SS with these settings. 
##### exp1-1 :  2 bidders in sealed SP auction 

![image.png](https://intranetproxy.alipay.com/skylark/lark/0/2023/png/229273/1684750047534-be0b737b-54e3-4c82-b9b9-d9f47822d667.png?x-oss-process=image/format,png#clientId=u3dfce8ba-0c3b-4&from=paste&height=480&id=uec23385d&originHeight=480&originWidth=640&originalType=binary&ratio=1&rotation=0&showTitle=false&size=1229090&status=done&style=none&taskId=u00dfbf1f-c1f4-47b3-9661-43235023fa0&title=&width=640)<br />Fig1. Wallet-game simulated results with 2 bidders in U[0,10] with second price auction. 

From the experiment results,  the simulated results is nearly the same towards the theoritical results. The distance may due to the usage of discrete space, and from the results, the bidder's strategy may overbid or lowerbid within 1 unit. (See [4]) 

##### exp1-2 :  multiple bidders (Common Value)
We then explore the effect of multiple bidders and we plot the results when 3,4,5 bidders are involved in the Wallet-game.<br />![image.png](https://intranetproxy.alipay.com/skylark/lark/0/2023/png/229273/1684750284644-20ec8523-29c1-4c58-a4b1-cf66b9499de1.png?x-oss-process=image/format,png#clientId=u4a085d5b-89a6-4&from=paste&height=480&id=u3cd157a0&originHeight=480&originWidth=640&originalType=binary&ratio=1&rotation=0&showTitle=false&size=1229090&status=done&style=none&taskId=u8690c946-e96f-46b1-9c0c-47d659e9906&title=&width=640)<br />Fig2. Wallet-game simulated results with 3 bidders in U[0,10] with second price auction. <br />![image.png](https://intranetproxy.alipay.com/skylark/lark/0/2023/png/229273/1684751233836-44dddef5-db5c-4449-ab6f-913ffc01ed45.png?x-oss-process=image/format,png#clientId=u4a085d5b-89a6-4&from=paste&height=480&id=u369210df&originHeight=480&originWidth=640&originalType=binary&ratio=1&rotation=0&showTitle=false&size=1229090&status=done&style=none&taskId=uf88466bd-039c-4bd0-b4c0-896ca5deab4&title=&width=640)<br />Fig3. Wallet-game simulated results with 4 bidders in U[0,10] with second price auction. 

With the increase of the agent number ,the bidding strategy turns to higher as the total expect revenue increased. From the convergence, there exist a bayesian nash equilibrium as all the agent policies are stable. 


##### exp1-3 :  value advantage
We validate the results on a large distribution. <br />![image.png](https://intranetproxy.alipay.com/skylark/lark/0/2023/png/229273/1684813372908-4cb4b068-64cf-4cc3-b703-f231032e7b7b.png?x-oss-process=image/format,png#clientId=u4a085d5b-89a6-4&from=paste&height=480&id=u6569c7ed&originHeight=480&originWidth=640&originalType=binary&ratio=1&rotation=0&showTitle=false&size=1229090&status=done&style=none&taskId=u8db08b59-fc7b-481d-8eb9-0d22d80687f&title=&width=640)<br />Fig. Wallet-game simulated results with 2 bidders in U[0,100] with second price auction. 

![image.png](https://intranetproxy.alipay.com/skylark/lark/0/2023/png/229273/1684814449038-d91686b3-25f2-447c-a8f5-e0dcce6deee9.png?x-oss-process=image/format,png#clientId=u4a085d5b-89a6-4&from=paste&height=480&id=u555e79e5&originHeight=480&originWidth=640&originalType=binary&ratio=1&rotation=0&showTitle=false&size=1229090&status=done&style=none&taskId=u2be3fee2-c167-43e3-a3a8-8041dd2d56f&title=&width=640)<br />Fig. Wallet-game simulated results with 3 bidders in U[0,100] with second price auction. 

From the experiment results,  the simulated results is nearly the same towards the theoritical results where a pure nash equilibrium exists as the symmetric bidding policies are illustrated. The slight differences between experimental results and theoritical results lie on the overbidding or lowerbidding in some cases, which is mentioned in [4,5] . 

Compared with the results in ![](https://intranetproxy.alipay.com/skylark/lark/__latex/9bd34494aae03605a36cd47a7adc7d16.svg#card=math&code=U%5Csim%5B0%2C10%5D&id=xU4yY), though larger training time is required, the final results are closer to the theoritical results. 

#### Conclusion
We implement theortical setting in AI4SS and explore possible equilibriums with provided MARL simulations.


### Setting 2. Symmetric agent in FP 
#### Experiment setting 
##### exp1-1 :  2 bidders in sealed FP auction 
We first validate the results in the **Wallet-game  **with 2 bidders with discrete valuation sampled from Uniform distribution ![](https://intranetproxy.alipay.com/skylark/lark/__latex/19fa58097401a8abe4bb347163258d84.svg#card=math&code=F_i%3DU%5B0%2C10%5D&id=pWexD) where we apply smart agent to simulate the bidder's behaviors and explore his optmial bidding policy. 
##### exp1-2 : ablation study : bidders 
We then increase bidder number to explore possible bidding policy. We test agent learned policy when the bidder number is 3, 4, 5. All bidders' valuation is sampled from uniform distribution ![](https://intranetproxy.alipay.com/skylark/lark/__latex/19fa58097401a8abe4bb347163258d84.svg#card=math&code=F_i%3DU%5B0%2C10%5D&id=N48B5).
##### exp1-3 : ablation study : valuation range  
We then enlarge the **Wallet-game  **with 2 bidders with discrete valuation sampled from Uniform distribution ![](https://intranetproxy.alipay.com/skylark/lark/__latex/415f0367b8abeb083d29aebbc052fc47.svg#card=math&code=F_i%3DU%5B0%2C100%5D&id=zDwvv) where we apply smart agent to simulate the bidder's behaviors and explore his optmial bidding policy. 

#### runnable script
The runnable script is recorded in the "./Equilibrium_Explore/script/run_cv_exp3_first_price.sh"<br />The detailed shell command is displayed below as well, 
```shell



# exp1-1
python Explore_3_CV.py --mode 'exp1_1' --mechanism 'first_price' --folder_name 'common_value_fp'

# exp 1-2
python Explore_3_CV.py --mode 'exp1_2' --mechanism 'first_price' --player_num 3 --folder_name 'common_value_fp'
python Explore_3_CV.py --mode 'exp1_2' --mechanism 'first_price' --player_num 4 --folder_name 'common_value_fp'
python Explore_3_CV.py --mode 'exp1_2' --mechanism 'first_price' --player_num 5 --folder_name 'common_value_fp'

#exp 1-3

python Explore_3_CV.py --mode 'exp1_3' --mechanism 'first_price' --player_num 2 --folder_name 'common_value_fp'
python Explore_3_CV.py --mode 'exp1_3' --mechanism 'first_price' --player_num 3 --folder_name 'common_value_fp'

```

#### Theoritical results
Based on the computed results on [6], the symmetric monotonic equilibrium bidding strategy ![](https://intranetproxy.alipay.com/skylark/lark/__latex/94951ed72502bd4a2e6f93a5233548b2.svg#card=math&code=b%5E%2A%28s_i%29&id=ZaZEN) is ![](https://intranetproxy.alipay.com/skylark/lark/__latex/37810040c2c4950b2c33325af82a5dc6.svg#card=math&code=b%5E%2A%28x%29%20%3Dv%28x%2Cx%29-%5Cint_%7Bx_%7Bmin%7D%7D%5E%7Bx_%7Bmax%7D%7DL%28%5Calpha%7Cx%29%20%5Cfrac%7Bd%7D%7Bd%5Calpha%7Dv%28%5Calpha%2C%5Calpha%29d%5Calpha%2C%20where&id=E4C8p)<br />![](https://intranetproxy.alipay.com/skylark/lark/__latex/bb9fbdc4da63773e613a4fba2967294f.svg#card=math&code=%20L%28%5Calpha%7Cx%29%20%3Dexp%28-%5Cint_%5Calpha%5Ex%20%5Cfrac%7Bf%28s%7Cs%29%7D%7BF%28s%7Cs%29%7Dds%29&id=On8qt)

In other words, a bidding shading strategy is performed to avoid "winners curse".

#### Experiment results
We display our exploration with MARL in AI4SS with these settings. 
##### exp1-1 :  2 bidders in sealed FP auction
##### ![image.png](https://intranetproxy.alipay.com/skylark/lark/0/2023/png/229273/1684821013386-d82c76fc-af54-46a6-a355-3906d2d632dc.png?x-oss-process=image/format,png#clientId=u4a085d5b-89a6-4&from=paste&id=u5756be34&originHeight=480&originWidth=640&originalType=binary&ratio=1&rotation=0&showTitle=false&size=1229090&status=done&style=none&taskId=u70125679-6a92-4be3-98ac-81cb0ffd963&title=)
Fig. Wallet-game simulated results with 2 bidders in U[0,10] with first price auction. 

From the results, we illustrate that all bidders perform a bidding shading strategy.

##### exp1-1 :  multiple bidders in sealed FP auction
We then explore the bidder number effects in sealed FP auction with common value. 

![image.png](https://intranetproxy.alipay.com/skylark/lark/0/2023/png/229273/1684821199278-c3200bda-561e-46b2-8996-b19d137956ec.png?x-oss-process=image/format,png#clientId=u4a085d5b-89a6-4&from=paste&id=u3341a17e&originHeight=480&originWidth=640&originalType=binary&ratio=1&rotation=0&showTitle=false&size=1229090&status=done&style=none&taskId=u1a470597-87f7-41f6-80f5-c3dadc2b4fb&title=)<br />Fig. Wallet-game simulated results with 3 bidders in U[0,10] with first price auction. 

![image.png](https://intranetproxy.alipay.com/skylark/lark/0/2023/png/229273/1684821222673-73652e43-a473-4be2-a5ff-f8f8e2143320.png?x-oss-process=image/format,png#clientId=u4a085d5b-89a6-4&from=paste&id=u010ea5fc&originHeight=480&originWidth=640&originalType=binary&ratio=1&rotation=0&showTitle=false&size=1229090&status=done&style=none&taskId=u6e6e8919-a96a-4d71-9721-ef8fa811474&title=)<br />Fig. Wallet-game simulated results with 4 bidders in U[0,10] with first price auction. 

When the agent number increase, similar symmetric equilibrium arise as below, all with bidding shading strategies to avoid winner curse.


##### exp1-3 :  value advantage
We validate the results on a large distribution. 

![image.png](https://intranetproxy.alipay.com/skylark/lark/0/2023/png/229273/1684821277928-a43664c6-5b99-4af1-a853-249f688ae34f.png?x-oss-process=image/format,png#clientId=u4a085d5b-89a6-4&from=paste&id=u4f99f09d&originHeight=480&originWidth=640&originalType=binary&ratio=1&rotation=0&showTitle=false&size=1229090&status=done&style=none&taskId=ue7a4f1a2-a9e0-419e-888a-6c5a7c025a6&title=)<br />Fig. Wallet-game simulated results with 2 bidders in U[0,100] with first price auction

![image.png](https://intranetproxy.alipay.com/skylark/lark/0/2023/png/229273/1684821392614-8c5d5306-dfdc-4cf4-a39b-cf77facb734a.png?x-oss-process=image/format,png#clientId=u4a085d5b-89a6-4&from=paste&id=u2f188545&originHeight=480&originWidth=640&originalType=binary&ratio=1&rotation=0&showTitle=false&size=1229090&status=done&style=none&taskId=u126c90cd-e303-401a-b67c-e058bc9a6ad&title=)<br />Fig. Wallet-game simulated results with 2 bidders in U[0,100] with first price auction

Similar to the results in the FP,  the simulated results is nearly the same towards the theoritical results where a pure nash equilibrium exists as the symmetric bidding policies are illustrated. 

#### Conclusion


### Setting 3. Asymmetric agent
#### Experiment setting 

exp1-1:  Asymmetric Wallet game + advantage [5]

According to [3], when increase a presumed small private value advantage (![](https://intranetproxy.alipay.com/skylark/lark/__latex/8595470dd82e04cf9b68e39e02021201.svg#card=math&code=V_1%3Ds_1%2Bs_2%2B%5CDelta%20s_1%3B%20V2%3Ds_1%2Bs_2&id=XTvJn)) with SP aution, it essentially denotes the assymatric equilibrium and the theoretical results are studied in clock auction. <br />We apply a warm experiment in exp1-3 to validate the effect of value advantage where there exist a presumed small common value in the symmetric agent setting, where ![](https://intranetproxy.alipay.com/skylark/lark/__latex/c6241107d37ab17a0a31f9c528b9d938.svg#card=math&code=V_1%3Ds_1%2Bs_2%2B%5CDelta%20s_1%3B%20V_2%3Ds_1%2Bs_2&id=bSAsH), and ![](https://intranetproxy.alipay.com/skylark/lark/__latex/a9e99076fdbcaa42a2978bbd336bad18.svg#card=math&code=%5CDelta%20s_1%20%20%5Csim%20U%5B0%2C5%5D&id=li97z)

#### runnable script

#### Theoritical results

#### Experiment results

#### Conclusion

---

## Reference
 [1] [https://web.stanford.edu/~jdlevin/Econ%20286/Auctions.pdf](https://web.stanford.edu/~jdlevin/Econ%20286/Auctions.pdf)<br /> [2] Vickrey, William (1961) “Counterspeculation, Auctions and Competitive Sealed Tenders,” Journal of Finance, 16, 8—39.<br /> [3] Kagel J H, Levin D. Auctions: a survey of experimental research'[M]. University of Pittsburgh, 2014.([https://www.asc.ohio-state.edu/kagel.4/HEE-Vol2/Auction%20survey_all_1_31_15.pdf](https://www.asc.ohio-state.edu/kagel.4/HEE-Vol2/Auction%20survey_all_1_31_15.pdf))<br /> [4] Gonçalves R, Ray I. A note on the wallet game with discrete bid levels[J]. Economics Letters, 2017, 159: 177-179.<br />[5] Klemperer P. Auctions with almost common values: TheWallet Game'and its applications[J]. European Economic Review, 1998, 42(3-5): 757-769.<br />[6] [http://www.its.caltech.edu/~mshum/ec106/theory1.pdf](http://www.its.caltech.edu/~mshum/ec106/theory1.pdf)
