# General Modal Logical Risk Analysis Framework

> **This project is the third in a three-part research arc:**
>
> 1. Modal Logical Access Control for AV Sensor Networks
>    -- Kripkean access control logic applied to a generalized AV
>    sensor network architecture
> 2. Modal Logical Network Security Analysis (independent study)
>    -- Extension to general network security analysis
> 3. **General Modal Logical Risk Analysis Framework** (this repo)
>    -- Domain-general unification of Safety Studies and Cybersecurity
>    under a modal logical risk model

---

## Overview

This repository contains the formal paper for a modal logical framework
for risk analysis, applicable across Safety Studies and Cybersecurity.
The work proceeds from the observation that these two fields share a
fundamental concern with risk, but treat it differently: Safety Studies
favors empirically motivated failure rates, while Cybersecurity grounds
risk in the logical imminence of a threat derived from the kill chain.

The paper argues that prevailing stochastic accounts of risk -- centered
on expected value formulations -- are incomplete, and that a modal
account grounded in Kripkean possible-worlds semantics more accurately
captures our intuitions about risk. The key insight, following Pritchard
(2005-2022), is that when modal closeness and probability diverge, human
judgment tends to track modal closeness rather than probability. This
motivates a formal model in which risk is expressed as a function of
modal distance, impact level, and defensive security strength.

---

## Theoretical Foundations

The framework rests on a spatialized extension of the Kripke frame,
drawing on the convergence of modal logic and metric graph theory
(van Benthem 1989; 2010; Wolter and Zakharyaschev 2003). The Kripke
accessibility relation R is interpreted as an edge set, transforming
a Kripke frame into a directed weighted graph whose connections
express both qualitative attack paths and quantitative degrees of
access difficulty.

The core model is a quintuple:

```
(W, Ra, Ka, V, d)   where:
W       -- nonempty set of worlds (possible system states)
Ra      -- possibility relation (access channel) for each entity a, where a can induce state v from state w iff (w,v) in Ra
Ka      -- knowledge function assigning each entity a a degree of awareness of its network state
V       -- valuation function assigning propositional atoms to states
d       -- metric function assigning positive reals to state pairs, expressing degree of modal distance
```
---

## The Risk Model

Risk at a target state t (assessed from an actual state w0) is
expressed as a function of three components:

```
R(w0, t, S) = P(t) * I(t) * sigma(d_S(w0, t))
where:
d_S(w0, t)  -- weighted modal distance from w0 to t, with weights derived from security strength function S
P(t)        -- perspicaciousness score: a decreasing function of distance (low score implies low risk)
I(t)        -- impact level at target state t (e.g., severity of a CIA breach)
S(e or v)   -- security strength function assessed at an edge e or state v, reflecting defensive investments (encryption, segmentation, tamper resistance, etc.)
```

Modal distance d(w0, t) is defined as the infimum over all paths
between w0 and t:

```
d(w0, t) = inf{ sum of r(wi, wi+1) : w0 -> ... -> t }
```
satisfying non-negativity, triangle inequality, and symmetry.

Security measures are modeled as distance enhancements: adding
authentication steps, encryption layers, or access controls increases
modal distance and thus reduces the perspicaciousness score and
overall risk -- formally matching the intuition that safeguards make
adverse events modally more distant.

---

## Application: Secure Remote Vehicle Diagnostics

The framework is demonstrated on a mock-up of a secure wireless
diagnostic module transmitting OBD-II data from a vehicle unit
(Raspberry Pi B+ / PiCAN FD) to a remote client over WiFi and
Bluetooth Low Energy (BLE). Two transmission paths are modeled
and compared:

```
Path 1 (WiFi):   w0 -> probe -> auth (EAP/RADIUS/4-way handshake) -> encrypted transmission -> wN
Path 2 (BLE):    w0 -> BLE pairing -> ECDH key exchange -> encrypted transmission -> wN
```

The model is populated with:

- Security strength valuations S reflecting standard 802.11
  and BLE protections, and the absence of OBD-II tamper
  protection or TrustZone access control
- Knowledge cost functions reflecting the vehicle unit's
  possession of sensitive CAN data vs. the client's more
  limited awareness
- Impact valuations I(t) scaled to the sensitivity of
  transmitted vehicle diagnostics

Earlier sections demonstrate the framework on 802.11 open system
and EAP-TLS frame exchanges, showing quantitatively that the
addition of EAP authentication states increases modal distance
and reduces risk scores as expected.

---

## Key Properties and Extensions

The paper identifies several directions for extension:

**Epistemic relations:** An epistemic relation ~a between worlds
allows the expression of an attacker's ability (or inability) to
distinguish worlds -- formally capturing the effect of encryption
on an attacker's information state.

**Impact mitigation:** A compound impact function accounts for
scenarios in which a security measure does not prevent an attack
but mitigates its cost (e.g., tamper-resistant storage that
confounds access even after physical theft).

**Hyperparameter balancing:** A hyperparameter lambda can be
introduced into the security strength function to balance the
relative risk scores of transmission paths of unequal length,
preserving fidelity to protocol structure while accommodating
comparative evaluation.

**Automation and ML integration:** World enumeration and
valuation selection are computationally feasible to automate.
A Python implementation would facilitate coupling with deep
learning models (e.g., MLP) for hyperparameter fine-tuning
of the security strength, impact, and distance functions.

---

## Relation to Prior Work

This framework emerged from a progression of related projects.
The modal logical access control framework for AV sensor networks
established the Kripkean formal apparatus and the inference rule
methodology. The independent network security study extended this
to general network topologies. The risk framework unifies both
lines of work under a single formal structure, and is motivated
by the observation -- first encountered in early coursework --
that risk is the concept that bridges Safety Studies and
Cybersecurity, and that a modal account is more general and
intuitive than prevailing stochastic approaches.

---

## Repository Structure
```
/
+-- README.md
+-- paper/
    +-- modal_risk_framework.pdf     <- Formal paper
```

---

## Note on Implementation

No implementation currently exists. The paper concludes with
an explicit direction for future work: a Python encoding of
the model that would allow for automated world enumeration,
valuation selection, and hyperparameter tuning via deep
learning integration. This is an active area of development.

---

## Standards and Literature

Key references:

- Pritchard, D. (2005-2022) -- Modal account of risk and
  epistemic luck
- van Benthem, J. (1989; 2010) -- Spatialized modal distance
- Wolter, F. and Zakharyaschev, M. (2003) -- Completeness
  and decidability results for modal distance
- Kutz, O. (2004) -- Logics of distance
- Hansson, S.O. (2022) -- Risk taxonomy
