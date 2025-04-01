(🕙Some Figures take time to load)
# 🌟Q1:

- In section 3.1:
  - Is *t* mistaken for *K*?
  - Why initial state can always converge to steady-state distribution? In fact, consider a deterministic Markov process  
    $p_{11} = 0, p_{12} = 1, p_{21} = 1, p_{22} = 0 \ (w = 0)$  
    and initial distribution $\{p_1 = 1, p_2 = 0\}$, *it will not approach the steady-state distribution*  
    $p_1 = 0.5, p_2 = 0.5$ as $t \to \infty$.
  - What is *L* in equation 4?
- $\delta_t = 0$ not necessarily mean that the diffusion process reaches equilibrium and diffusion no longer grows.  
  For the Markov process above with $1 > w > 0$, it is easy to verify that $K_0 = 0$ but $T^{(k)}$ never converges in a finite time.  $\delta_t = 0$ should only be stated as an empirical metric.

# 🌟A1:
We sincerely appreciate your careful and insightful comments. Based on your feedback, we have revised Section 3.1 (See Figure Re.1) accordingly and would like to clarify the following points in detail:

1. **On the confusion between *t* and *K***:  
   Thank you for the observation. In Eq. (3), we use $t$ to denote the number of diffusion steps in the general process $\mathbf{h}(t)$, while $K$ represents the maximum diffusion depth (i.e., truncation point). In practice, we compute up to a constrained step $\beta = 2K_0$, derived from an empirical saturation condition. We agree that this could be better clarified in the text and will update the notation to avoid ambiguity between temporal index $t$ and the upper diffusion limit $K$ or $\beta$.

2. **On convergence to steady-state**:  
   You're absolutely right that *not all* deterministic Markov processes converge to a unique steady-state. In our paper, the statement regarding the convergence is made **under the assumption of stochastic and ergodic random walks** (e.g., personalized PageRank diffusion), where convergence is guaranteed under mild conditions (irreducibility and aperiodicity).  
   We acknowledge that this assumption was implicit and will revise the text to state it explicitly. Also, we have now stated more clearly that the condition $\delta_t = 0$ (Eq. 7) is used **as an empirical convergence indicator**, rather than implying formal convergence in all cases.

3. **Clarification on Equation (4) and meaning of *L***:  
   Thank you for pointing this out. In Equation (4), $L_{\mathbf{h}_{\text{steady}}} = 0$ refers to the **graph Laplacian operator** applied to the steady-state node distribution. That is, if $\mathbf{L} = \mathbf{I} - \mathbf{T}_{\text{sym}}$, then $\mathbf{L} \mathbf{h}_{\text{steady}} = 0$ indicates that $\mathbf{h}_{\text{steady}}$ is in the kernel of the Laplacian, i.e., it is constant over connected components — a standard result in graph diffusion theory. We’ll revise the text to include a clearer definition of $\mathbf{L}$ and this interpretation.

4. **On the use of $\delta_t = 0$ as an equilibrium indicator**:  
   As you rightly noted, $\delta_t = 0$ does not always imply exact convergence, particularly in edge cases or deterministic processes. In our work, we only use it as a practical stopping criterion to avoid further computational cost when the change in support (non-zero entries) stagnates. We now emphasize that it is a heuristic and does not indicate convergence in the strict mathematical sense.

These revisions clarify the theoretical assumptions and enhance the conceptual precision of our approach. They are reflected in Section 3.1 of the revised manuscript (See Figure Re.1). Once again, thank you for your valuable feedback😊.

<div align="center"><strong>Figure Re.1: Revised Section 3.1

<img width="590" alt="image" src="https://github.com/user-attachments/assets/700e63dd-08d9-4d1f-b705-9519a2c2e863" /></strong></div>

# 🌟Q2:
The authors should discuss more on the influence of FIFR. My intuition is the FIFR increases the diffusion between nodes with similar features. Why it helps increase performance and reduce over-diffusion?

# 🌟A2:
Thank you for this insightful question regarding the role and effect of Feature Information Flow Routing (FIFR). We have expanded the discussion in our revised manuscript to clarify this component (See Figure Re.2).

FIFR is specifically designed to regulate the structure of the diffusion pathway by dynamically weighting the importance of edges based on feature similarity, rather than purely relying on structural connectivity. Your intuition is correct: FIFR enhances diffusion between nodes with similar features, but importantly, it suppresses diffusion between dissimilar nodes, thereby reducing the chance of irrelevant or noisy message propagation.

This mechanism helps in two key ways:

**Performance Boost through Feature-Aware Routing**:  
   In many graphs, structural proximity does not always imply semantic similarity. FIFR adjusts the edge-level diffusion strength by learning a routing coefficient (denoted $\gamma_{ij}$) based on feature similarity. This aligns the diffusion process more closely with the task-relevant signals, leading to improved node classification accuracy, especially in heterophilic or noisy graphs.

**Mitigating Over-Diffusion via Routing Constraints**:  
   Over-diffusion often occurs when information spreads too widely without control, diluting unique node-level features. By prioritizing semantically similar nodes and suppressing redundant or uninformative propagation, FIFR introduces a natural *sparsity and directionality* in the diffusion pattern. This limits feature homogenization and retains meaningful local context, addressing the over-smoothing issue.

<div align="center"><strong>Figure Re.2: Revised Section 3.2 about the interpretability of FIFR.

<img width="531" alt="image" src="https://github.com/user-attachments/assets/18f52b70-234f-4979-8871-523d3fda16e5" />
</strong></div>

# Q3:
Most experimental designs are sound. A possible improvement can be adding larger datasets like ogbn-arxiv.

# A3:
We fully agree that evaluating on larger and more challenging datasets like OGB is important. In response, we attempted to run CGDConv on **OGB-arxiv**, and encountered a critical scalability bottleneck due to the size of the diffusion matrix. Specifically, computing the PPR-based diffusion matrix involves inverting a $169,343 \times 169,343$ adjacency matrix, which requires approximately ****118GB of memory****. This leads to a silent failure on standard hardware: the process terminates without error output, as shown in the attached runtime log (see Figure Re.3). The Python process exceeds the memory limit, the operating system will directly terminate the process without throwing a Python exception. The "adj matrix over" you see is the last successfully executed step, after which the process is killed by the system.

**This issue is not specific to our model.** It arises from the matrix inversion operation commonly used in diffusion-based models like GDC and ADC, which are also not scalable to such graph sizes without approximation. We acknowledge that handling large-scale graphs remains a crucial open challenge and plan to investigate efficient and scalable solutions (e.g., sparsified diffusion, Monte Carlo estimation) in future work.  


<div align="center"><strong>Figure Re.3: Silent crash when running the OGB dataset.
   
<img width="786" alt="image" src="https://github.com/user-attachments/assets/9cccef12-1d51-405f-830f-84b56579c43f" /></strong></div>


# Q4:
Font size of Figure 3 should be increased.
# A4: 
We appreciate your suggestion regarding Figure 3. Although the figure is presented in vector format—ensuring resolution is preserved regardless of scaling—we acknowledge that the compressed layout and small font size may still have hindered readability during review. Due to space constraints in the main manuscript, we had reduced the font and spacing to maintain flow. 

However, we fully agree with your concern, and in the revised version, we will relocate Figure 3 to the appendix and enlarge its font size and layout for improved clarity and ease of reading.
