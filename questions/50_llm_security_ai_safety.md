# AI Safety & Security for Large Language Models – Question Candidates

This list presents 12 interview questions that progressively deepen into security and safety challenges unique to LLM deployment.

1. **Basic Threats:** What are prompt injections and why are they a primary security concern for LLM-powered applications?
2. **Output Filtering:** Explain the difference between blocklists and classifier-based safety filters for LLM outputs.
3. **Data Privacy:** How can retrieval-augmented generation inadvertently leak private documents, and how might you mitigate this?
4. **Model Updates:** Describe risks that arise when hot-patching a production LLM with newly fine-tuned weights.
5. **PII Redaction Pipelines:** Outline a pipeline to scrub personally identifiable information (PII) from user prompts before they reach the model.
6. **Rate & Abuse Controls:** Which monitoring metrics would you instrument to detect automated abuse (e.g., spam generation) in an LLM API?
7. **Supply-Chain Security:** What threats exist in third-party model weights or open-source checkpoints, and how do you validate them before use?
8. **Jailbreak Robustness Testing:** Propose a red-team methodology for systematically discovering jailbreak prompts.
9. **Adversarial Embeddings:** Explain how adversarially crafted vector store entries can manipulate downstream LLM answers in RAG systems.
10. **Model Extraction Attacks:** How can an attacker approximate proprietary model weights via query access, and which defenses reduce leakage?
11. **Fine-Tuning Backdoors:** Discuss how malicious gradients can implant backdoors during RLHF or SFT, and detection strategies.
12. **Alignment vs. Security Trade-offs:** Analyze a scenario where stricter refusal rates reduce productivity; how would you find the optimal balance?
