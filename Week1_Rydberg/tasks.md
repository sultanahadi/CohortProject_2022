![CDL 2022 Cohort Project](../CDL_logo.jpg)
# Project 1: solutions


## Modelling Rydberg atom arrays
The foundation of today’s neutral-atom quantum computers is [Rydberg atoms](https://arxiv.org/abs/2002.07413). Briefly, Rydberg atoms are highly-excited atoms (e.g. Rubidium or Strontium) that interact with each other on the scale of a few micrometres. A controlled laser pulse can excite an atom into a quantum state with a large principal quantum number (i.e. a Rydberg state) that is quasi-stable. 
The binary nature of a Rydberg atom’s ground state $|g \rangle $ and excited state $|r \rangle $ defines a two-level system, and such atomic states can be used to build qubit-based quantum computers.

Rydberg atoms are held into a physical location in the array by optical tweezers. This allows a high degree of tunability of the inter-atomic interactions.
We will consider Rydberg atom arrays on a interaction graph, where the Hamiltonian has the general form
$$H =  \frac{\Omega}{2} \sum_{i \in V}\sigma_i^x  - \delta \sum_{i \in V} n_i + \sum_{ i < j } V_{ij} n_i n_j$$
Here, $n_i = 1/2 \left({ I - \sigma_i^z }\right) = |r \rangle \langle r|_i$ is called an occupation operator.  

The first term (Rabi frequency) is off-diagonal, and analogous to a transverse field: $\sigma^x = |g \rangle \langle r| + |r \rangle \langle g|$.  The second term (detuning) favours all sites being occupied with an excitation, while the final (interaction) term penalizes simultaneously occupied pairs. 

The interaction takes the form $V_{ij} = \Omega\left(\frac{R_b}{r_{ij}}\right)^6$, where $R_b$ is a parameter called the [blockade](https://queracomputing.github.io/Bloqade.jl/dev/tutorials/1.blockade/main/#blockade) radius, and the distance between atoms located at vertices $i$ and $j$ is $r$.
The computational basis we will be working in is the occupation basis, $| g \rangle $ and $| r \rangle $.
The eigenstates of the Rydberg occupation operator are $n_i | g \rangle_j = 0$ and $n_i | r \rangle_j  = \delta_{i,j} |r \rangle_j$, for all sites $i$ and $j$.


The goal of the simplest Rydberg atom quantum computer is to prepare the groundstate wavefunction of some target Hamiltonian, defined by the ratio of the detuning to the Rabi frequency, and the interaction $V_{ij}$, which is in turn defined by the geometry of the optical tweezer lattice $r_{ij}$, and the blockade radius.  

The computer can easily be prepared with all atoms are in their groundstate, that is, every single-atom state is $| 0 \rangle $.  

Then, final (target) states are prepared by slowly tuning $\delta(t)/\Omega(t)$ as a function of time using the adiabatic state preperation protocol.

Let's explore state preparation protocols on this neutral atom quantum computer. Below are series of Tasks and optional Challenges for each team to attempt.

## Task 1: Adiabatic state preparation with Bloqade



Our first task will be to prepare an [experimentally motivated](https://www.nature.com/articles/nature24622) ordered state consisting of alternating ground and Rydberg states in a one-dimensional (1D) chain, the so-called $\mathbb{Z}_2$ state: $$ |\psi \rangle = | g \hspace{1mm} r \hspace{1mm} g \hspace{1mm} r \hspace{1mm} g \hspace{1mm} r  \cdots \rangle.$$
To get us started, we will use the open-source simulation software [Bloqade](https://github.com/QuEraComputing/Bloqade.jl) to prepare the $\mathbb{Z}_2$ state of a one-dimensional chain of 9 atoms with open boundary conditions.  In order to prepare this state, follow the Bloqade [tutorial](https://queracomputing.github.io/Bloqade.jl/dev/tutorials/2.adiabatic/main/#Preparation-of-Ordered-States-in-1D):
generate the pulse/detuning sequence, specify the atomic position, then starting in the groundstate, simulate the time evolution of a quantum state under the Schrödinger equation.  
Plot the average occupation on each site as a function of time.

In addition, calculate the expectation value of $\langle \sigma^x_i \rangle$ and gap $E_1 - E_0 = \Delta E$ between the Rydberg Hamiltonian's groundstate and first excited state.  What does the latter imply about the viability of the adiabatic protocol for this type of quantum computer? Can you determine how it scales with increasing array size?


## Task 2: Larger arrays with the Blockade Approximation

What is the largest 1D array that you can simulate for the full Rydberg Hamiltonian, with exact time evolution as above? 
What implications does this have for quantum advantage, particularly in light of the experiments mentioned above?

Next, in order to significantly reduce the size of the state space required to solve Rydberg problems, one can eliminate states that violate the blockade constraint (e.g. more than one atom being excited within $R_b$).  Implement the [blockade approximation](https://queracomputing.github.io/Bloqade.jl/dev/tutorials/2.adiabatic/main/#Emulation-in-the-Blockade-Subspace) and justify it; i.e. when do you expect it to be valid? You can find more detailed explaination about this approximation in [Rydberg Blockade](https://queracomputing.github.io/Bloqade.jl/dev/tutorials/1.blockade/main/) section.

Repeat your study of 1D arrays, and find the largest system for which you can adiabatically prepare the $\mathbb{Z}_2$ state.
What is the largest 2D array for which you can adiabatically prepare the checkerboard phase?  Has quantum advantage for 2D arrays already [been achieved](https://www.nature.com/articles/s41586-021-03582-4)?

## Task 3: MIS

Maximum Independent Set (MIS) is a combinatorial optimization problem that is naturally suited to implementation on a Rydberg atom quantum computer. Specifically, the Rydberg blockade implies that two atoms cannot be both excited to the Rydberg state $| r \rangle$ if they are close to each other. In MIS, the independent set constraint means that two vertices on a graph cannot be both in the independent set when they are connected by an edge. Thus, one can consider atoms in the Rydberg state as vertices in an independent set. Read more about the MIS problem [here](https://queracomputing.github.io/Bloqade.jl/dev/tutorials/4.MIS/main/#mis-tutorial).

Following the Bloqade tutorial [script](https://github.com/QuEraComputing/Bloqade.jl/blob/master/examples/4.MIS/main.jl), solve the $4 \times 4$ diagonal-connected unit-disk grid graphs (DUGG) problems using the adiabatic approach. How large of an array can Bloqade solve?  Attempt to solve for larger square DUGGs using your tensor network approach. Try different pulse parameterization and compare which is better, and explore how smoothing affects the performance.

## Task 4: Business applications

For industrial application, we have looked at Protein-folding. Protein is a sequence of amino-acid. In the protein folding problem, one is given a sequence on 1D, unfolded protein, based on its interactions with each other and outside, one has to find how it becomes functions, folds to a 3D shape. Protein folding is crucial for designing new therapeutics. In general, protein folding is NP-hard. Quantum promises to provide speedup for an optimization problem. Protein folding is an optimization problem, and we expect that quantum computers can speed up the process.  There has been ongoing work to experimentally show speed for useful application of quantum speed up for the optimization is one of them.  There has been some work of designing Quantum algorithms for protein folding problems, that has indicated speed up on quantum computers. 

We can write the problem  Hamiltonian [[1,2,3]](https://arxiv.org/abs/1811.00713,https://www.nature.com/articles/srep00571.pdf,https://arxiv.org/pdf/2205.06084.pdf) as
$$ H_I= H_{\text{pair}}+H_{\text{penalty}}$$
where $ H_{\text{pair}}$ encodes the interaction between different acids, valid interaction, and $H_{\text{penalty}}$ penalizes the unwanted amino-acid fold, and the the full Hamiltonian is the sum of a non-interacting and the interacting part
$$\label{pro} H= H_{\text{free}}+ H_{\text{pair}}+H_{\text{penalty}}.$$
The ground state of $H$ gives us the string with valid protein folds. To solve this problem with Rydberg-Hamiltonian, we have to map it that Hamiltonian. The  Rydberg-Hamiltonian is 
$$ H= \frac{\Omega}{2}\sum_{i\in V}\sigma_i^x-\delta\sum_{i\in V}n_i+\sum_{i>j}V_{ij}n_in_j.$$ 
We map 
$$ H_{\text{free}}\rightarrow -\delta\sum_{i\in V}n_i, $$
$$ H_{\text{pair}}+H_{\text{penalty}}\rightarrow \sum_{i>j}V_{ij}n_in_j. $$

The problem is to start in the ground-state of $\sum_{i\in V}\sigma_i^x$, 
$$ |\psi_0\rangle$$,
and then addibaitcally go the ground sate of the full  Hamiltonain
$$ |\psi_g\rangle= e^{-i\int_{t_0}^{t_f}\left(\frac{\Omega}{2}f(t)\sum_{i\in V}\sigma_i^x-g(t)(\delta\sum_{i\in V}n_i+\sum_{i>j}V_{ij}n_in_j)\right)dt}|\psi_0\rangle.$$

The function $f(t)$ and $g(t)$, for the adibatic protocoal, is define such that at $t_0$ the state $|\psi_0\rangle,$  and at time $t_f,$ one gets $|\psi_g\rangle.$  Note $e^{-i\int_{t_0}^{t_f}\left(\frac{\Omega}{2}f(t)\sum_{i\in V}\sigma_i^x-g(t)(\delta\sum_{i\in V}n_i+\sum_{i>j}V_{ij}n_in_j)\right)dt}$ can be discretized. 

The state $|\psi_g\rangle$ is populated with differnet protein folds and the one with high-probality is conidered the stable folds. It is multi objective optimization, and solves for a solution space in a multivariate set up. We can also encode molecular-docking as an optimization in the the Ryberg-Hamiltonian. In 



Finally, use your simulations to solve a groundstate encoding problem for an industrial application.  You may come up with your own, or look at one of the examples given in this recent [preprint](https://arxiv.org/abs/2205.08500): i.e. Portfolio optimization (example VI), Network immunization (example VII), or Task scheduling (example XII). Set up a problem, and solve it on as large a graph as possible using adiabatic state preparation protocols. Link this to your [Business Application](./Business_Application.md).

### Once you are comfortable with the above tasks, you may turn to one or more of the below optional **Challenges** for any time remaining in your project. 

## Challenge 1: Proteing-folding with MPS

The protein-folding problem that is mapped to the Ryberg-Hamiltonian can be simulated using MPS. The time evolution can be discretized and further approximated by Troterization. One can check against the performance Ryberg devices aginst MPS simulation. Since MPS is considered the best classical simulatuion method for simulating the ground state of 1D Hamiltonian and if MPS fails to do do what Ryberg devices do, then one can calim Qunatum advantage with Ryberg devices. We are the first to suggest to simulate Proteing-folding with MPS. 



## Challenge 2:
The protien-folding is already implemented using QAOA. It is intersting to lo 


It would be intersting to simulate the same problem with MPS, RH, QAOA, QA. 






