# Lab T1 — Modelling & Transfer Functions of a Robot Joint — Report

**Course:** M-408-01 Robot Control & Feedback Systems · MSESR (TSI)
**Topic 1 lab:** DC-motor robot joint → transfer function → closed proportional position loop
**Student name: Arina**
**Student ID: 62264**
**Course repo:** `<https://github.com/cizevskaarina-byte/Lab1_Robot-Control-and-Feedback-Systems/tree/main>` · **Code path:** `/labT1/`
**Date:**

---

## 1. Aim

In this lab I built a transfer-function model of a robot joint, G(s) = Km / (s(tau s + 1)), with Km = 4.0 and tau = 0.25 s.
Around it I closed a feedback loop with a proportional controller Kp = 1.0 and a sensor gain H = 0.5 in the feedback path.
A transfer-function model is needed because it lets me calculate in advance how the joint will behave (stability, overshoot, final position) without testing on the real robot..

---

## 2. Method

1. **Model** - I took Part 1 code from lab and hanged the numbers to Km = 4.0 and tau = 0.25. G(s) is written as two coefficient lists: numerator [4.0] and denominator [0.25, 1.0, 0.0]. Open loop poles are 0 and -4 . Pole at 0 is integrator(the angle accumulates, so without feedback the joint drifts),and the pole at −4 is the motor lag.
2. **Reduce** - I used series and feedback functions from Part 1 without changes. Series connects controller and the joint one after another (multiplies the polynomials) and feedback closes loop with sensor gain H = 0.5 using T(s) = G / (1 + GH).
3. **Standard form** - I divided the closed-loop denominator 0.25s^2 + s + 2 by 0.25 to get s^2 + 4s + 8 and compared it with s^2 + 2 _ zeta _ wn*s + wn^2. So wn^2 = 8, wn = 2.8284 rad/s, and 2 * zeta _ wn = 4, zeta = 0.7071. In the code: wn = sqrt(Kp _ Km \* H / tau).
4. **Step metrics** -I used the formulas from Part 1.
5. **Simulate** — I used RK4 simulation from Part 1 with time step dt = 0.0001 s for 12 s. Because joint settles at dc = 2 instead of 1, I replaced 1.0 with dc in motion equation, in overshoot (measured as a percentage of dc) and in the 2% settling band.

> **Why the joint settles at `r/H` and not at `r`**.
> Joint settles at the r/H and not at the r because sensor report only half of the real angle (H = 0.5). Controller stops when measured value H\*y equals reference r = 1, which happens when real angle is y = r/H = 1/0.5 = 2.

---

## 3. Results

### 3.1 Headline numbers

| Quantity                                  | Value                  |
| ----------------------------------------- | ---------------------- |
| Open-loop poles                           | `0` , `4`              |
| Forward path num / den                    | `[4]` / `[0.25, 1, 0]` |
| Closed-loop num / den                     | `[4]` / `[0.25, 1, 2]` |
| Closed-loop poles                         | `-2 ± 2j`              |
| Natural frequency `wn` (rad/s)            | `2.8284`               |
| Damping ratio `zeta`                      | `0.7071`               |
| Overshoot `Mp` — analytic / simulated (%) | `4.32` / `4.32`        |
| Peak time `tp` — analytic / simulated (s) | `1.5708` / `1.5708`    |
| 2 % settling time — estimate (s)          | `2.0000`               |
| DC gain `T(0)` / simulated `y(inf)`       | `2.000` / `2.0000`     |

### 3.2 Reading the result (3–5 sentences)

Closed-loop poles are −2 ± 2j. Loop is stable because both poles lie to the left of zero (the real part −2 is negative). Imaginary part is not zero (±2j), so loop is underdamped and oscillates (joint overshoots target by the 4.32 % (zeta = 0.7071) before settling , peak time (1.5708 s) and final value (2.0) match the analytic values, so both methods agree. ) T(0) = 2 means a steady-state error (joint settles at 2 instead of 1, because sensor gain is H = 0.5. To make joint settle exactly at the target, sensor must be calibrated so that H = 1.

---

## 4. LLM Co-development Log

| #   | Prompt you gave the AI                              | What it claimed / produced                                                                                               | The check you ran                                                                            | Your decision (accepted / corrected / rejected) — in your own words |
| --- | --------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------ | -------------------------------------------------------------------------------------------- | ------------------------------------------------------------------- |
| 1   | Guide me through Part 1 step by step and explain it | Gave Part 1 code from the lab, one cell at a time with explanation                                                       | Compared every output with expected output in the lab: all 5 checkpoints matched             | Accepted                                                            |
| 2   | My simulated overshoot is 8.64 %, not 4.32 %        | Hinted that 8.64 = 2 × 4.32                                                                                              | I found that I forgot to divide by dc; after the fix: 4.32 %                                 | Corrected my own mistake                                            |
| 3   | Independent pole check                              | Gave a template with one blank                                                                                           | I first put dc in blank and got True only by coincidence; changed it to Kp·Km·Hg, still True | Corrected my own mistake                                            |
| 4   | Help me write the report with correct English       | Asked me questions. I answered in Russian, the AI translated into English and wrote the Method sentences from my answers | I checked that I can explain every sentence                                                  | Accepted |
| 5 | Report template attached to Lecture 1 was for Lab T6, not Lab T1 | Adapted  template for Lab T1(same sections, but with the Lab T1 tasks, numbers and file names) | I compared  sections with list in  Lab T1 brief (joint_tf.py, part2.py, report.md, Reality-Check, Co-development Log) | Accepted |

---

## 5. Reality-Check evidence (the code actually runs)

```text
G num = [4.]  G den = [0.25 1.   0.  ]
open-loop poles = [-4.+0.j  0.+0.j]
forward num,den = [4.] [0.25 1.   0.  ]
closed  num,den = [4.] [0.25 1.   2.  ]
closed-loop poles = [-2.-2.j -2.+2.j]
wn = 2.8284 rad/s,  zeta = 0.7071
Mp = 4.32 %,  tp = 1.5708 s,  ts(2%) approx = 2.0000 s
DC gain T(0) = 2.0000
sim overshoot = 4.32 %  (analytic 4.32 %)
sim peak time = 1.5708 s  (analytic 1.5708 s)
sim 2% settling = 2.1080 s,  final value y(12s) = 2.0000
```

**Independent pole check:**

```text
char poly roots = [-2.-2.j -2.+2.j]
match: True
```

- [x] `/labT1/joint_tf.py` runs and reproduces all five Part 1 checkpoints.
- [x] `/labT1/part2.py` runs top-to-bottom.
- [x] The numbers in section 3 are copied from the output blocks above, not entered manually.
- [x] The closed-loop poles are confirmed by the independent `1 + KpGH = 0` route.
- [x] I did **not** "fix" the code to force `T(0) = 1`.
- [x] I can explain every line of my code without the AI.

---

## 6. Declaration

I confirm this report and the code at `/labT1/` are my own work; any AI assistance is logged
honestly in section 4; and the results in section 3 are the real output of the code in section 5.

**Signed:** · **Date:**
