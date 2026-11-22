# Lab T1 — Modelling & Transfer Functions of a Robot Joint — Report

**Course:** M-408-01 Robot Control & Feedback Systems · MSESR (TSI)
**Topic 1 lab:** DC-motor robot joint → transfer function → closed proportional position loop
**Student name:**
**Student ID:**
**Course repo:**  `<link to your GitHub repo>`  ·  **Code path:** `/labT1/`
**Date:**

---

## 1. Aim

*In 2–3 sentences, state what you built: the joint model (`G(s) = Km / (s(tau s + 1))`,
`Km = 4.0`, `tau = 0.25 s`), the loop you closed around it (proportional controller `Kp = 1.0`,
sensor gain `H = 0.5` in the feedback path), and why a transfer-function model is needed before
you can design a position controller.*

---

## 2. Method

1. **Model** — how you wrote `G(s)` as numerator/denominator polynomials in NumPy, and what its open-loop poles mean physically.
2. **Reduce** — your `series` and `feedback` helpers from Part 1, and the formula `T(s) = G/(1+GH)` they implement (state the sign convention: negative feedback).
3. **Standard form** — how you matched the closed-loop denominator to `s^2 + 2*zeta*wn*s + wn^2` to get `wn` and `zeta`.
4. **Step metrics** — the analytic formulas you used for overshoot `Mp`, peak time `tp`, 2 % settling time and DC gain `T(0)`.
5. **Simulate** — the RK4 integration of the closed-loop step response (state your step size) and how you measured overshoot, peak time and steady-state value off the curve.

> One sentence must explain, in your own words, **why the joint settles at `r/H` and not at `r`**.

---

## 3. Results

### 3.1 Headline numbers

| Quantity | Value |
|---|---|
| Open-loop poles | `__` , `__` |
| Forward path num / den | `[__]` / `[__, __, __]` |
| Closed-loop num / den | `[__]` / `[__, __, __]` |
| Closed-loop poles | `__ ± __j` |
| Natural frequency `wn` (rad/s) | `__.____` |
| Damping ratio `zeta` | `0.____` |
| Overshoot `Mp` — analytic / simulated (%) | `__.__` / `__.__` |
| Peak time `tp` — analytic / simulated (s) | `__.____` / `__.____` |
| 2 % settling time — estimate (s) | `__.____` |
| DC gain `T(0)` / simulated `y(inf)` | `__.____` / `__.____` |

### 3.2 Reading the result (3–5 sentences)

*Is the loop stable, and how do you know from the poles alone? Is it underdamped, critically
damped or overdamped, and what does that mean for the joint's motion? Do the simulated metrics
agree with the analytic ones? What does `T(0)` tell you about steady-state error with a sensor
gain `H = 0.5`, and what would you change to make the joint settle at the commanded angle?*

---

## 4. LLM Co-development Log

| # | Prompt you gave the AI | What it claimed / produced | The check you ran | Your decision (accepted / corrected / rejected) — in your own words |
|---|---|---|---|---|
| 1 | | | | |
| 2 | | | | |
| 3 | | | | |

---

## 5. Reality-Check evidence (the code actually runs)

```text
<paste the real output of part2.py here>
```

**Independent pole check:**

```text
<paste the output of your independent check here>
```

- [ ] `/labT1/joint_tf.py` runs and reproduces all five Part 1 checkpoints.
- [ ] `/labT1/part2.py` runs top-to-bottom.
- [ ] The numbers in section 3 are copied from the output blocks above, not entered manually.
- [ ] The closed-loop poles are confirmed by the independent `1 + KpGH = 0` route.
- [ ] I did **not** "fix" the code to force `T(0) = 1`.
- [ ] I can explain every line of my code without the AI.

---

## 6. Declaration

I confirm this report and the code at `/labT1/` are my own work; any AI assistance is logged
honestly in section 4; and the results in section 3 are the real output of the code in section 5.

**Signed:**  ·  **Date:**
