---
layout: default
title: Obaida Research and Publications
---

<div id ="research">

<h3> Publications</h3>


<ol>
  <li><b>[2018 ACM SIGSIM PADS]</b> Mohammad Abu Obaida, Jason Liu, Gopinath Chennupati, Nandakishore Santhi and  Stephan Eidenbenz, Parallel Application Performance Prediction Using Analysis Based Models, 2018 Principles of Advanced Discrete Simulation (PADS), Rome,Italy, 23 May 2018.
  </li>
  <li><b>[2017 Wintersim]</b> Mohammad Abu Obaida and Jason Liu, Simulation of HPC Job Scheduling and Large-Scale Parallel Workloads, 2017 Winter Simulation Conference (WSC 2017), Las Vegas, NV, December 2017.
  </li>
  <li><b>[2017 SIMUTOOLS]</b> Mohammad Abu Obaida and Jason Liu, On Improving Parallel Real-Time Network Simulation for Hybrid Experimentation of Software Defined Networks, 10th EAI International Conference on Simulation Tools and Techniques (SIMUTOOLS 2017), Hong Kong, September 2017.
  </li>
  <li><b>[2016 ACM SIGSIM PADS]</b> Kishwar Ahmed, <b>Mohammad Obaida</b>, Jason Liu, Stephan Eidenbenz, Nandakishore Santhi, Guillaume Chapuis, An Integrated Interconnection Network Model for Large-Scale Performance Prediction, Proceedings of the 2016 annual ACM Conference on SIGSIM Principles of Advanced Discrete Simulation, pp 177-187, ACM SIGSIM PADS, May 15, 2016.
  </li>
   <li><b>[2014 GREE]</b> Jason Liu, <b>Mohammad Abu Obaida</b>, Fernando Dos Santos, Toward PrimoGENI Constellation for Distributed At-Scale Hybrid Network Test, 2014 Third GENI Research and Educational Experiment Workshop (GREE). March 19, 2014. 
  </li>
   <li><b>[2011 IJCA]</b> <b>Mohammad Abu Obaida</b>, Md. Sanaullah Miah and Md. Abu Horaira, Random Early Discard (RED-AQM) Performance Analysis in Terms of TCP Variants and Network Parameters: Instability in High-Bandwidth-Delay Network. International Journal of Computer Applications 27(8):40-44, August 2011.
  </li>
   <li><b>[2011 IJCA]</b> <b>Mohammad Abu Obaida</b>, Md. Jakir Hossain, Momotaz Begum and Md. Shahin Alam. Article: Multilingual OCR (MOCR): An Approach to Classify Words to Languages. International Journal of Computer Applications 32(1):46-53, October 2011.
  </li>

<br><br>

<h3> Paper Abstracts:<h3>

<p><h4>[2018 ACM SIGSIM PADS]</b> Mohammad Abu Obaida, Jason Liu, Gopinath Chennupati, Nandakishore Santhi and  Stephan Eidenbenz, Parallel Application Performance Prediction Using Analysis Based Models, 2018 Principles of Advanced Discrete Simulation (PADS), Rome,Italy, 23 May 2018.</h4>

Parallel application performance models provide valuable insight 
about the performance in real systems. Capable tools providing fast, accurate,  
and comprehensive prediction and evaluation of high-performance computing (HPC) 
applications and system architectures have important value.  This paper presents 
<i>PyPassT</i>, an analysis based modeling framework built on static program analysis 
and integrated simulation of target HPC architectures.  More specifically, 
the framework analyzes application source code written in C with OpenACC 
directives and transforms it into an application model describing its computation 
and communication behavior (including CPU and GPU workloads, memory accesses, 
and message-passing transactions).  The application model is then executed 
on a simulated HPC architecture for performance analysis.  Preliminary experiments 
demonstrate that the proposed framework can represent the runtime behavior of 
benchmark applications with good accuracy.
</p>


<p><h4> [2017 Wintersim] Mohammad Abu Obaida and Jason Liu, Simulation of HPC Job Scheduling and Large-Scale Parallel Workloads, 2017 Winter Simulation Conference (WSC 2017), Las Vegas, NV, December 2017. </h4>
The paper presents a simulator designed specifically for evaluating
job scheduling algorithms on large-scale HPC systems.  The simulator
was developed based on the Performance Prediction Toolkit (PPT), which
is a parallel discrete-event simulator written in Python for rapid
assessment and performance prediction of large-scale scientific
applications on supercomputers.  The proposed job scheduler simulator
incorporates PPTs application models, and when coupled with the
sufficiently detailed architecture models, can represent more
realistic job runtime behaviors.  Consequently, the simulator can
evaluate different job scheduling and task mapping algorithms on the
specific target HPC platforms more accurately.
</p>


<p><h4>[2017 SIMUTOOLS] Mohammad Abu Obaida and Jason Liu, On Improving Parallel Real-Time Network Simulation for Hybrid Experimentation of Software Defined Networks, 10th EAI International Conference on Simulation Tools and Techniques (SIMUTOOLS 2017), Hong Kong, September 2017.</h4>
Real-time network simulation enables simulation to oper- ate in real time, and in doing so allows experiments with simulated, emulated, and real network components acting in concert to test novel network applications or protocols. Real-time simulation can also run in parallel for large-scale network scenarios, in which case network tra c is repre- sented as simulation events passed as messages to remote simulation instances running on di erent machines. We note that substantial overhead exists in parallel real-time simula- tion to support synchronization and communication among distributed instances, which can signi cantly limit the perfor- mance and scalability of the hybrid approach. To overcome these challenges, we propose several techniques for improving the performance of parallel real-time simulation, by elimi- nating parallel synchronization and reducing communication overhead. Our experiments show that the proposed tech- niques can indeed improve the overall performance. In a use case, we demonstrate that our hybrid technique can be readily integrated for studies of software-de ned networks.
</p>

<p>
<h4> [2016 ACM SIGSIM PADS] An Integrated Interconnection Network Model for Large-Scale Performance Prediction</h4>

Interconnection network is a critical component of high-performance computing architecture and application co-design. For many scientific applications, the increasing communication complexity poses a serious concern as it may hinder the scaling properties of these applications on novel architectures. It is apparent that a scalable, efficient, and accurate interconnect model would be essential for performance evaluation studies. In this paper, we present an interconnect model for predicting the performance of large-scale applications on high-performance architectures. In particular, we present a sufficiently detailed interconnect model for Crays Gemini 3-D torus network. The model has been integrated with an implementation of the Message-Passing Interface (MPI) that can mimic most of its functions with packet-level accuracy on the target platform. Extensive experiments show that our integrated model provides good accuracy for predicting the network behavior, while at the same time allowing for good parallel scaling performance.
</p>

<p> <h4> [2014 GREE] Toward PrimoGENI Constellation for Distributed At-Scale Hybrid Network Test</h4>
PrimoGENI provides a GENI aggregate interface through which experimenters can launch large-scale network experiments on GENI resources consisting of both simulated network and real instances of network applications directly running on either virtual or physical machines. Real network traffic generated by the network applications can be introduced into the simulated network in real time and be subjected to proper delays and losses according to the simulated network conditions. To leverage the previous PrimoGENI prototype activities, PrimoGENI Constellation is a newly launched project, which will focus specifically on facilitating distributed at-scale hybrid experiments for real-world high-impact applications. In this paper, we provide an overview of the major achievements of PrimoGENI, and more importantly, discuss the ongoing efforts in PrimoGENI Constellation aiming to achieve the full potential of the hybrid network experiment approach. The main thrusts of PrimoGENI Constellation include: 1) supporting at-scale network experiments potentially distributed on different types of GENI resources in accordance with the GENI experiment workflow, 2) focusing on target applications supporting prominent and high-impact future Internet research, and 3) building the user community through extensive education and research training, and online archives of experiment results and user experiences.
</p>

<p> <h4>[2011 IJCA] Random Early Discard (RED-AQM) Performance Analysis in Terms of TCP Variants and Network Parameters: Instability in High-Bandwidth-Delay Network</h4>
Conventional congestion control methods (e.g. DROP TAIL) discards all received packets after the queue is full moreover results in low-network performance. To address this problem, RED was proposed to improve the performance of TCP connections. As a queue management mechanism, it drops packets in the considered router buffer to adjust the network traffic behavior according to the queue size. In application, TCP Variants (Reno, NewReno, Vegas, Fack and Sack1) show oscillatory curve of packet reception if RED is considered for queuing, besides, some variants out performs in receiving packets over different network parameters that this paper analyzes and finds out. However, an increase in link capacity (with the resulting increase of per-flow bandwidth) will cause significant degradation in TCP’s performance, irrespective of the queuing scheme used. Hence the network is prone to instability with the rise in the number of High-bandwidth-delay product that is also attended to in this paper.
</p>

<p>
<h4> [2011 IJCA] Multilingual OCR (MOCR): An Approach to Classify Words to Languages </h4>
There are immense efforts to design a complete OCR for most of the world’s leading languages, however, multilingual documents either of handwritten or of printed form. As a united attempt, Unicode based OCRs were studied mostly with some positive outcomes, despite the fact that a large character set slows down the recognition significantly. In this paper, we come out with a method to classify words to a language as the word segmentation is complete. For the purpose, we identified the characteristics of writings of several languages and utilized projecting method combined with some other feature extraction methods. In addition, this paper intends a modified statistical approach to correct the skewness before processing a segmented document. The proposed procedure, evaluated for a collection of both handwritten and printed documents, came with excellent outcomes in assigning words to languages.
</p>

