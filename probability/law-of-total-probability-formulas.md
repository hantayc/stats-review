# Law of Total Probability: Formula Reference

## 1. The setup: a partition

Events B₁, B₂, …, Bₙ form a **partition** of the sample space if:

- They are **mutually exclusive**: Bᵢ ∩ Bⱼ = ∅ for i ≠ j
- They are **exhaustive**: B₁ ∪ B₂ ∪ … ∪ Bₙ = S (one of them must happen)
- Each has P(Bᵢ) > 0

Their probabilities sum to 1: P(B₁) + P(B₂) + … + P(Bₙ) = 1

---

## 2. Core formulas

**Multiplication rule** (the building block)

    P(A ∩ B) = P(A | B) · P(B)

**Law of Total Probability (general form)**

    P(A) = Σᵢ P(A | Bᵢ) · P(Bᵢ)
         = P(A|B₁)P(B₁) + P(A|B₂)P(B₂) + … + P(A|Bₙ)P(Bₙ)

**Two-case form** (partition is just B and its complement Bᶜ)

    P(A) = P(A | B) · P(B) + P(A | Bᶜ) · P(Bᶜ)
    where P(Bᶜ) = 1 − P(B)

**Conditional version** (everything also conditioned on C)

    P(A | C) = Σᵢ P(A | Bᵢ ∩ C) · P(Bᵢ | C)

**Continuous version** (conditioning on a continuous random variable X with density f)

    P(A) = ∫ P(A | X = x) · f(x) dx

---

## 3. Bayes' Theorem (the "reverse" question)

LTP gives the denominator. Use it when the question asks *which case caused what you saw*.

    P(Bⱼ | A) = P(A | Bⱼ) · P(Bⱼ) / P(A)
              = P(A | Bⱼ) · P(Bⱼ) / Σᵢ P(A | Bᵢ) · P(Bᵢ)

Two-case form:

    P(B | A) = P(A|B)P(B) / [ P(A|B)P(B) + P(A|Bᶜ)P(Bᶜ) ]

---

## 4. Related: Law of Total Expectation

    E[X] = Σᵢ E[X | Bᵢ] · P(Bᵢ)        (discrete partition)
    E[X] = E[ E[X | Y] ]               (iterated expectation)

---

## 5. Solving recipe

1. **Name the target event A.** What are you finding the probability of?
2. **Find the partition.** Ask: *"What do I need to know first to make A easy?"* That unknown (which urn, which machine, rain or not, the die roll) is your partition B₁…Bₙ.
3. **Write the priors** P(Bᵢ). Check they sum to 1.
4. **Write the conditionals** P(A | Bᵢ) for each case.
5. **Multiply and add:** P(A) = Σ P(A | Bᵢ) P(Bᵢ).
6. **Reverse question?** If asked "given A happened, what's the chance it was Bⱼ?", divide that one branch by the total (Bayes).

**Tree diagram shortcut:** first branches = P(Bᵢ), second branches = P(A | Bᵢ).
Multiply along each path; add the paths that end in A.

---

## 6. Keyword cues

| Wording in problem | What it gives you |
|---|---|
| "40% of items come from machine B" | Prior P(B) |
| "of those from B, 3% are defective" | Conditional P(D \| B) |
| "what fraction overall / at random" | Use LTP: P(D) |
| "given it's defective, probability it came from B" | Use Bayes: P(B \| D) |
| "false positive rate" | P(+ \| no disease) |
| "sensitivity" / "true positive rate" | P(+ \| disease) |
| "specificity" | P(− \| no disease) = 1 − false positive rate |

---

## 7. Common mistakes

- **Mixing up P(A | B) and P(B | A).** "5% of spam contains X" is P(X | spam), not P(spam | X).
- **Partition doesn't cover everything.** Forgetting the complement case (e.g., "no rain").
- **Overlapping cases.** Partition pieces must not overlap or you double-count.
- **Priors don't sum to 1.** Quick sanity check that catches many errors.
- **Forgetting to divide in Bayes.** The numerator alone is P(A ∩ Bⱼ), not P(Bⱼ | A).
- **Sanity check:** P(A) must land between the smallest and largest P(A | Bᵢ). It's a weighted average.
