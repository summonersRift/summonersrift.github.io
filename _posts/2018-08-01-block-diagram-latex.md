---
published: true
---
## Draw block dragram in latex

We would see a simple example of drawing block diagram in latex


#### Code Snippet

{{ "{% highlight bash" }}%}
% Author: Marek Fiser, and Mohammad Abu Obaida
\documentclass[tikz, border=10pt]{standalone}
\usetikzlibrary{arrows}
\begin{document}
\begin{tikzpicture}
[->,>=stealth',shorten >=1pt,auto,node distance=3cm,
  thick,main node/.style={circle,fill=blue!20,draw,
  font=\sffamily\Large\bfseries,minimum size=10mm},GPU node/.style={circle,fill=green!20,draw,
  font=\sffamily\Large\bfseries,minimum size=10mm}]

  \node[main node] (B0) {B0};
  \node[main node] (B1) [right of=B0] {B1};
  \node[GPU node] (G1) [below of=B0] {G1};
  \node[GPU node] (W1) [right of=G1] {W1};
  \node[GPU node] (W2) [right of=W1] {W2};
  \node[GPU node] (G11) [right of=W2] {G1'};
  \node[main node] (B2) [right of=B1] {B2};
  \node[main node] (B3) [right of=B2] {B3};

  \path[every node/.style={font=\sffamily\small,
  		fill=white,inner sep=1pt}]
  	% Right-hand-side arrows rendered from top to bottom to
  	% achieve proper rendering of labels over arrows.
    (B0) edge [bend left=1] node[right=1mm] {} (B1)
    (B1) edge [bend right=10] node[right=1mm] {} (G1)
    (G1)%edge [loop above] node {PrRd/-} (G1)
        edge [bend left=1] node[right=1mm] {} (W1)
    (W1)edge [loop above] node {iter1} (G1)
        edge [bend left=1] node[right=1mm] {} (W2)        
    (W2)edge [loop above] node {iter2} (G1)
        edge [bend left=1] node[right=1mm] {} (G11)
    (G11) edge [bend right=10] node[right=1mm] {} (B2)      
  	% Left-hand-side arrows rendered from bottom to top to
  	% achieve proper rendering of labels over arrows.
    (B2) edge [bend left=5] node[left=1mm] {} (B3);
\end{tikzpicture}
\end{document}

{{ "{% endhighlight" }}%}



Output pdf:<br>
<img src="{{ '/assets/img/touring.jpg' | prepend: site.baseurl }}" alt="">
