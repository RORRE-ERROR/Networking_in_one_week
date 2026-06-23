# 🌐 Networking in One Week — WIA1005 Study Pack

A complete, self-paced study pack built from your lecture slides (Chapters 1–13), your uploaded tutorials (Packet Tracer labs), and the style of your uploaded quiz banks.

The material is deliberately **paced for learning, not cramming** — each chapter explains the *why* before the *what*, with worked examples and memorable exam tips. Take it one chapter at a time.

---

## 📁 How this pack is organized

```
Networking_in_one_week/
├── README.md                     ← you are here (the 7-day plan)
├── Review_Before_Exam.md         ← ⭐ the all-in-one final review (read last)
├── Graded_Labs_Guide.md          ← step-by-step for the graded labs (LT-01, LT-02, L2)
│
├── 01_Network_Communication/
│   ├── Notes.md                  ← teaching notes (learn here)
│   └── Quiz.md                   ← exam-style quiz (test yourself)
├── 02_Network_Protocols_and_NOS/
│   ├── Notes.md
│   └── Quiz.md
│   ... (one folder per chapter, 01 → 13) ...
└── 13_Build_and_Secure_Small_Network/
    ├── Notes.md
    └── Quiz.md
```

**Each chapter folder** has two files:
- **`Notes.md`** — read this first. Clear teaching notes with 💡 exam tips, tables, command blocks, and worked examples.
- **`Quiz.md`** — exam-style questions (CCNA format, like your uploaded banks). Answers are marked `[x]` with explanations. **Cover the answers and test yourself first.**

At the top level, **`Review_Before_Exam.md`** condenses every chapter into one high-yield revision sheet — use it the day before the exam.

---

## 🗓️ The 7-Day Plan

The 13 chapters are grouped into 7 days. Each day: **read the Notes → do the Quiz → note what you got wrong.**

| Day | Chapters | Theme | Why grouped |
|---|---|---|---|
| **Day 1** | 1, 2 | Foundations: what a network is + how devices talk (OSI/TCP-IP, IOS basics) | The vocabulary everything else builds on |
| **Day 2** | 3, 4 | Getting on the wire + moving data (Ethernet/ARP, IP + TCP/UDP) | Layers 1–4 — the bottom of the stack |
| **Day 3** | 5 | **IPv4 addressing & subnetting** | The single biggest exam topic — give it a full day |
| **Day 4** | 6, 7 | IPv6 + the Application layer | Modern addressing + the services users see |
| **Day 5** | 8, 9 | Switching + VLANs & inter-VLAN routing | How a switched LAN really works |
| **Day 6** | 10, 11 | DHCP + Routing (static & dynamic) | Automatic addressing + getting between networks |
| **Day 7** | 12, 13 | Wireless LAN + Build & Secure a small network | Wrap-up + security, then the final review |

> After Day 7, work through **`Review_Before_Exam.md`** and re-take any quiz where you scored below ~80%.

---

## 🧪 Using the Packet Tracer labs

Your `ITN_..._Source_Files/` and `SRWE_..._Source_Files/` folders contain the `.pka` labs. Each chapter's **Notes.md → 🧪 Hands-on** section lists the labs that match it. Open them in Cisco Packet Tracer and *do the configuration yourself* — typing the commands is the fastest way to make them stick for the practical/exam.

---

## ✅ Study tips that actually work

1. **Active recall beats re-reading.** Always attempt the quiz with answers covered.
2. **Subnetting is muscle memory** — do a few subnet problems *every day*, not just on Day 3.
3. **Type the commands.** Reading `ip route ...` is not the same as configuring it in Packet Tracer.
4. **Track your misses.** Keep a running list of questions you got wrong; that list *is* your final-review priority.
5. **Don't cram Chapter 13 security as trivia** — connect each attack to its mitigation.

Good luck — you've got a full week and a full map. Work the plan. 💪
