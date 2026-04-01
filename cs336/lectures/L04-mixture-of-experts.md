# L04: Mixture of Experts

## Summary
- MoE is not “different experts for different topics.” It is sparse activation over multiple FFN copies.
- The payoff is compute efficiency: more total parameters, similar active FLOPs per token.
- Routing is the hard part. Top-K gating is now the default shape, but it collapses without balancing.
- DeepSeek-style MoE is the lecture’s endgame: shared experts, fine-grained experts, and load-balancing tricks.
- Expert parallelism is the systems reason MoE matters. It gives you a natural sharding boundary.

## Key Concepts
- **Sparse FFNs:** the attention block stays dense; the MLP/FFN becomes the MoE component.
- **Router:** a lightweight scoring function that assigns each token to a small subset of experts.
- **Top-K routing:** the dominant pattern in modern MoE systems. Token-choice routing wins because it is simple and workable.
- **Expert parallelism:** each expert or group of experts can live on a different device.
- **Load balancing:** without it, a small number of experts absorb nearly all traffic and the rest die.
- **Token dropping:** if a batch overloads an expert, systems may cap traffic and drop excess tokens.

## Non-Obvious Insights
- The lecture is blunt about the naming: “mixture of experts” is misleading. These are not semantic domain specialists.
- MoE often looks good because it moves along the compute/parameter frontier. Same active FLOPs, more capacity.
- The router can be surprisingly dumb and still work. Hashing-based routing comes up as evidence that the system is doing more than semantic clustering.
- The biggest failure mode is not quality first; it is utilization collapse. A dead-expert MoE is just a more expensive dense model with worse training dynamics.
- DeepSeek-V3’s “auxiliary loss-free” balancing is not magic. It still uses balancing pressure, just encoded differently.
- The lecture’s real thesis is that MoE is as much a distributed-systems decision as it is a modeling decision.

## Connections
- Connects directly to the next systems lectures: GPU kernels, tensor parallelism, pipeline parallelism, and distributed training.
- MoE is the bridge between architecture and cluster topology. Once experts are sharded, network layout starts to matter.
- The same balancing instinct reappears later in distributed training: utilization is a first-class objective, not an implementation detail.
- DeepSeek is treated as a lineage, not a one-off model. The architecture stays stable while the surrounding system becomes more sophisticated.

## Referenced Papers
- **Switch Transformers** (Fedus, Zoph, Shazeer, 2022) - canonical top-K MoE routing and load-balancing.
- **GShard** (Lepikhin et al., 2020) - early large-scale conditional computation and automatic sharding.
- **DeepSeek-V2: A Strong, Economical, and Efficient Mixture-of-Experts Language Model** - modern open MoE reference point.
- **DeepSeek-V3 Technical Report** - the lecture’s main architectural endpoint for expert parallelism and balancing.

## Timestamps
- **0:06** - lecture framing: MoE as a now-critical topic.
- **0:21** - DeepSeek-V3 as the target system to decode.
- **1:35** - the bad intuition: MoE is not domain-specialized experts.
- **2:42** - FFN blocks become multiple experts plus a router.
- **17:15** - routing design space: token choice, expert choice, global assignment.
- **22:11** - the convergence to top-K routing.
- **32:01** - DeepSeek’s shared expert plus fine-grained expert idea.
- **48:43** - Switch Transformer load-balancing loss.
- **52:16** - DeepSeek-V3’s auxiliary-loss-free balancing.
- **1:00:00** - expert parallelism as a systems primitive.
- **1:10:10** - walkthrough of the DeepSeek MoE architecture.

Source: YouTube transcript extracted with `summarize --extract --timestamps --plain` from https://www.youtube.com/watch?v=LPv1KfUXLCo.
