---
layout: default
title: Projects | Obaida
---

<div id="projects">
Github: <a href ="https://github.com/summonersrift">github.com/summonersrift</a>
<h3>Ph.D. Work and Research Projects</h3>
<ol>
  <li><b>Performance Prediction Toolkit (PPT)</b> (2015-2018) 
         Parallel application and system performance prediction toolkit implemented on Simian parallel discrete-event simulator. 
         PPT is a collaborative project with Los Alamos National Laboratory (LANL), NM.  
         It models large parallel interconnections such as Torus, Fat-tree, DragonFly, InfiniBand and provides easily extensible modeling constructs.
         PPT also includes fairly detailed processor, memory-cache, GPU-accelerator models that an application model can make use of, for making accurate predictions.
         <br><b>Languages/Technologies</b>: Python, Simian. 
         <br>GitHub: <a href="https://github.com/lanl/PPT">https://github.com/lanl/PPT</a>.</li>


  <li><b>PyPassT</b> (2017-2018). 
         HPC Simulation model construction using static program analysis. A number of advanced program analysis techniques has been used to identify program performance.  
         <br><b>Languages/Tools</b> Java, C, CUDA, LLVM, Cetus compiler, Compass, PPT.</li>


  <li><b>Workload Scheduling Simulator</b> (2016-207). I developed a simulator designed specifically for evaluating job scheduling algorithms on large-scale HPC systems. 
         It allows large-scale HPC cluster workload modeling with job scheduling, task mapping and application execution. 
         The simulator can evaluate different job scheduling and task mapping algorithms on the specific target HPC platforms more accurately. 
         This allows prediction and measurement of application performance variability in different runtime conditions.
         <br>GitHub: <a href="https://github.com/summonersrift/ppt-sched">https://github.com/summonersrift/ppt-sched</a>.</li>

  <li><b>SDNScaleNet</b> (2016)
         Network emulation using Linux namespaces, OVS w/ Pox Controller. 
         A cloud infrastucture or data-center network is programmed as a graph or tree in python. 
         Large models can also be built by supplying parameters. 
         Alternatively, the network topology can be read from a file. 
         The network can be deployed as containers with links. Consider connecting Docker / LXC / Linux Namespaces with virtual ip-links/veth-pairs. 
         To enable software defined networking SDNScaleNet uses Open vSwitch(OVS) and pox-controller. 
         <br><b>Languages/Technologies</b>: Python, Bash(Shell), ip-links, veth-pairs, Linux Network namespaces(ip netns), LXC containers.</li>

  <li><b>Distributed Simulator</b> (2016) 
         Efficient distributed hybrid <b>real-time</b> cloud, data-center, and large network simulator . 
         The real-time simulator PRIME which was  using a simgle core in linux is capable opf processing <b>300,000 events/second</b>. 
         The simulator makes full use of the event rate of the PRIME real-time simulator. 
         This particular implementation achieved <b>5x throughput</b> increase compared to all previous parallel real-time simulation implementation.
         <b>Languages/Tools</b>: C, C++, PRIME, SDNScaleNet.</li>

  <li><b>PrimoGENI Constellation</b> (2013 – 2015) Developed distributed hybrid network experimentation on NSF GENI testbed.
         <br><b>Languages/Tools</b>: C++, Java, Bash, Apache-mina(state message transfer), Jsch(port forwarding), Perl, LXC, InstaGENI, ExoGENI, PrimoGENI, VM instances.
         <br>GitHub: <a href="https://github.com/netsym/primogeni">https://github.com/netsym/primogeni</a>.</li>

  <li><b> RED/XCP</b> (2011): Studied TCP variants (vegas, reno etc.) extensibly for congestion control algorithms in NS-2 and 
          recommended the best variant to use for a certain network architecture with unique bandwidth-delay scenarios. 
          The models for NS-2 simulator was written in Tcl and the GB of results or the traces were parsed in Perl.</li>

<br><h3>Idea Validation and Class Projects</h3>

  <li><b>Trading Bot</b> (2018) Cryptocurrency trader using financial indicators. 
         The algorithm was successfully deployed on Coinbase/GDAX exchange. 
         The trader used Python-pandas to handle data structures and <i>python-stockstats</i> to calculate the indicators 
          which a trader can mix and match to find the right signals. 
         We used <i>CCXT</i> library to make the API calls, which makes the program independent of the exchange and their API. 
         We also built an early prototype on QuantConnet platform which provides excellent backtesting capability. 
         <br><b>Tools/Languages:</b> Python, python-stockstats, CCXT, QuantConnect.</li>

  <li><b>Arbitrator Bot</b> (2018) A cryptocurrency atbitrator that exploits ask/bid price differences in different market (order depths). 
         For example, if there exists an arbitration <i>BTC/LTC</i> and <i>ETH/LTC</i> markets or the orders, 
         the trader finds the arbitrations for all such coin pairs against the base pairs. 
         Few miliseconds, prototype arbitration processing time achieved for hundreds of markets on cryptocurrency exchange Binance. 
         <br><b>Languages/Tools</b>: Python; Python-binance API.</li>

  <li><b>Car Parking Reservation</b> (2015) Object oriented design pattern and software development class project. 
         A car parking reservation with search, book, incident reporting, payment submission features implemented in Java and MySQL. 
         <br><b>Languages/Tools</b>: Java, MySQL, Eclipse-Papyrus.
         <br>GitHub: <a href="https://github.com/summonersRift/carParkingReservation">https://github.com/summonersRift/carParkingReservation</a>.</li>

</ol>

</div>
