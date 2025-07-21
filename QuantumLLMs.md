# Quantum Large Language Models via Tensor Network Disentanglers

**Authors**: Borja Aizpurua, Saeed S. Jahromi, Sukhbinder Singh, and Román Orús  
**Affiliations**:  
1. Multiverse Computing, Spain  
2. Tecnun - University of Navarra, Spain  
3. Donostia International Physics Center, Spain  
4. Multiverse Computing, Toronto, Canada  
5. Ikerbasque Foundation for Science, Spain  

## Abstract

We propose a method to enhance the performance of Large Language Models (LLMs) by integrating quantum computing and quantum-inspired techniques. Specifically, we replace the weight matrices in Self-Attention and MLP layers with two variational quantum circuits and a quantum-inspired tensor network, such as a Matrix Product Operator (MPO). This allows classical LLM functionality via tensor network disentanglers and MPO decomposition. With deeper quantum circuits and higher bond dimensions, our method improves accuracy while maintaining low memory overhead.

---

## 1. Introduction

- **Transformer & LLMs**: Transformers [Vaswani et al.] are the core of modern LLMs like ChatGPT, LLaMA, and BERT.
- **Energy Cost Issue**: Training ChatGPT-3 cost ~$100M in electricity; this cost is doubling every 10 months.
- **Need for Compression**: Quantum-inspired tensor networks (TNs) offer promising compression with minimal accuracy loss.
- **Quantum Computing**: NISQ (Noisy Intermediate-Scale Quantum) devices enable use of variational quantum circuits for ML and optimization despite being error-prone.

> **Goal**: Merge quantum computing with classical LLMs by replacing weight matrices in deep layers with hybrid quantum circuits + tensor networks.

---

## 2. Main Idea

- Replace large **weight matrices** in Self-Attention and MLP layers with a **VQC + TN + VQC** architecture:
  - **VQC**: Variational Quantum Circuit
  - **TN**: Tensor Network (e.g., MPO)
- This hybrid setup captures more correlations, resulting in:
  - Higher accuracy
  - Lower memory usage
  - Polynomially scaling overhead (manageable)

### Architecture Comparison

#### (a) Classical Transformer  
Components:
- Input/Output Embeddings
- Positional Encoding
- Multi-Head Attention
- Feed Forward Network
- Layer Normalization and Add

#### (b) Quantum LLM (QLLM)  
Modifications:
- Replace weight matrices in deep layers with `VQC + TN + VQC`

---

## 3. From LLM to QLLM

### Conversion Steps:

1. **Decompose W with MPO**  
   - Apply CompactifAI algorithm to express W as an MPO.
   - Truncate bond dimension (χ) for memory efficiency.

2. **Compute Disentangler Circuits (U, V†)**  
   - Use 2-body unitary gates to remove entanglement.
   - One acts on input, one on output.
   - Based on Entanglement Renormalization (ER) and MERA.

3. **Build New MPO (MPO_new)**  
    $$
    \text{MPO}_{\text{old}} \approx U \times \text{MPO}_{\text{new}} \times V^\dagger
    $$
    
    And solving for the new MPO gives:
    
    $$
    \text{MPO}_{\text{new}} \approx U^\dagger \times \text{MPO}_{\text{old}} \times V
    $$
   - Use TEBD (Time-Evolving Block Decimation) to compute MPO_new.
   - Ensure χ_new ≤ χ_old.

> **Result**: A compressed, entanglement-reduced quantum-compatible weight operator.

---

## 4. Deployment and Optimization

- **Training**:
  - Use VQE-style optimization for U and V†.
  - Retrain MPO tensors using distributed TN techniques.
- **Deployment on Quantum Hardware**:
  - Encode input as quantum states (e.g., via GANs or TNs).
  - Sample output from qubit measurements.
- **Scalability**:
  - Deeper U, V† and higher χ yield better approximations and performance.
  
---

## 5. Discussion and Conclusions

- Currently evaluating performance on models like LlaMA.
- Can be combined with other compression methods:
  - Quantization
  - Distillation
  - Pruning
  - Low-rank factorization
- Early estimates suggest hundreds of qubits are sufficient.
- Matches IBM’s quantum hardware roadmap.
- May become one of the first practical applications of NISQ-era quantum computers.

---

## Acknowledgements

Supported by DIPC, Ikerbasque, Basque Government, Diputación de Gipuzkoa, EIC, IASBS, and Spanish Government. Special thanks to the team at Multiverse Computing.

---

## References (Selected)

1. ChatGPT, The Guardian (2022)  
2. A. Vaswani et al., *Attention is All You Need*, NeurIPS (2017)  
3. OpenAI, *Language Models are Unsupervised Multitask Learners* (2019)  
4. Meta, *LLaMA* (2023)  
5. Google, *BERT* (2018)  
6. The Economist, *The bigger-is-better approach to AI is running out of road* (2023)  
7. CompactifAI: *Extreme compression of large LLMs* (2024)  
8. J. Preskill, *Quantum Computing in the NISQ era* (2018)  
9. Peruzzo et al., *VQE on a photonic quantum processor* (2014)  
10–33. Further references include MERA, MPOs, quantum GANs, TEBD, and related tensor/quantum literature.

---

# Quantum LLMs via Tensor Network Disentanglers (Explained Simply)

**Authors**: Borja Aizpurua, Saeed S. Jahromi, Sukhbinder Singh, Román Orús  
**Published**: 2024

---

## 🚀 What is this paper about?

This paper explores a new way to improve Large Language Models (LLMs)—like ChatGPT or LLaMA—by combining:

- **Quantum computing**
- **Tensor network methods** (from physics)

The goal is to replace the large and expensive parts of LLMs (weight matrices) with a mix of:

- Quantum circuits  
- Tensor network compression

This allows models to be **smaller, more efficient**, and potentially **more accurate**, while using less memory.

---

## 🔍 Why is this important?

- LLMs are **very large** and **expensive** to run and train.
- Quantum processors are **getting better** and could soon handle meaningful tasks.
- Tensor networks are a powerful **compression tool** that keeps important information while reducing size.

> This work creates a **hybrid architecture** that mixes classical and quantum tools to get the best of both.

---

## 🧠 Main Idea: Replace Big Matrices with VQC + TN + VQC

LLMs have layers with big weight matrices. This paper proposes replacing each weight matrix with:

Each deep layer in the LLM is restructured as:

**Variational Quantum Circuit (VQC) ➜ Tensor Network (TN) ➜ Another VQC**

# Quantum LLMs via Tensor Network Disentanglers (Explained Simply)

$$
W \approx U \times \text{MPO}_{\text{new}} \times V^\dagger
$$

Where:

- $W$ = original weight matrix  
- $U$ and $V^\dagger$ = unitary quantum disentangling circuits  
- $\text{MPO}_{\text{new}}$ = compressed tensor network (Matrix Product Operator)

---


This structure can:
- Learn better correlations
- Save memory
- Still work within classical Transformer architectures

---

## 🧱 How They Do It: Step-by-Step

### 1. Start with a weight matrix (W) in an LLM
This could be from any deep layer, like attention or MLP.

### 2. Compress it with an MPO
Use a **Matrix Product Operator** to break W into smaller tensors. Truncate bond dimensions (like pruning less important values).

### 3. Disentangle with quantum circuits (U and V†)
- Apply one quantum circuit to the input side (U)
- Apply another to the output side (V†)
- These reduce the entanglement in W, making the compression more efficient

This gives:

### 4. Rebuild the new tensor + quantum layer
The **new compressed version (MPO_new)** + the **quantum gates (U, V†)** form the new layer of the LLM.

---

## 🧪 How Do You Train or Use This?

- You can start with any trained LLM (like BERT or LLaMA)
- Replace deep layers using this VQC+TN+VQC setup
- Optimize it:

  - Train the VQCs using a method like **VQE** (Variational Quantum Eigensolver)
  - Retrain MPO with classical tensor optimization methods

- On quantum hardware:
  - Input must be encoded into quantum states
  - Output must be sampled from qubits

> Accuracy improves as you:
> - Increase quantum circuit depth
> - Increase tensor bond dimensions

---

## 📊 Advantages of This Method

✅ Compresses LLMs without losing much accuracy  
✅ Can **improve** performance when you increase depth/dimensions  
✅ Works with today's LLMs (no need to start from scratch)  
✅ Uses **existing** quantum hardware capabilities (NISQ)

---

## 🧠 What’s a “Disentangler”?

A **disentangler** is a small quantum circuit that "untangles" complex data—kind of like simplifying a tangled pair of earphones.

Here, it helps reduce the complexity (entanglement) in the tensor network, allowing the model to be represented with less information.

---

## 💡 How is This Better Than Classical Compression?

- Classical compression only reduces size.
- This method:
  - Compresses
  - Adds **extra expressive power** using quantum circuits
  - Keeps models **accurate and efficient**

---

## 🔭 Future of This Idea

- Works even with quantum devices with just a few hundred qubits
- Can be combined with other tricks:
  - Quantization
  - Knowledge distillation
  - Low-rank factorization
- May become one of the **first real quantum AI applications**

---

## 🧾 Credits and Support

Supported by:

- Donostia International Physics Center (DIPC)
- Ikerbasque
- Basque Government
- European Innovation Council (EIC)
- Multiverse Computing
- Spanish Government

---

## 📚 Key References

- Vaswani et al., 2017 – Transformers ("Attention is All You Need")  
- OpenAI GPT Reports  
- CompactifAI (2024) – Previous LLM compression work  
- Preskill, 2018 – NISQ quantum computing  
- Vidal – Entanglement Renormalization & MERA

---



