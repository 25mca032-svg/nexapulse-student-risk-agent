# 🚀 NexaPulse – Autonomous Predictive Action Agent

An AI agent built in a Jupyter Notebook that **analyzes past + current data, predicts upcoming situations, and automatically plans & executes suitable actions**.

This demo applies the idea to **student performance**: the agent monitors students week by week, predicts who is at risk of failing the final exam, finds out *why*, and triggers the right interventions (alerts, extra practice, mentoring, guardian notice) before it is too late.

> ⚠️ All data in this project is **synthetic** (generated inside the notebook) and all actions run in **simulation mode**. No real student data is used and no real messages are sent.

---

## 🧠 How the agent works

```
Perceive → Predict → Diagnose → Plan → Act → Learn
```

| Step | What the agent does |
|------|---------------------|
| **Perceive** | Reads each student's attendance, quiz, assignment, study-hours and late-submission data up to the current week |
| **Predict** | Random Forest models estimate fail-risk (classifier) and expected final score (regressor) |
| **Diagnose** | Finds the reason: low attendance, falling quiz scores, late submissions, low study time |
| **Plan** | Chooses actions based on the reason and the risk level |
| **Act** | Sends student / teacher / guardian messages (simulated), with a cooldown to avoid spamming |
| **Learn** | After the semester, checks flag precision & recall and adjusts its risk threshold for the next semester |

## ✨ Features

- Past semester data used for training, current semester monitored **week by week**
- Early warning from **week 4** onward
- Reason-based, personalised actions (not one-size-fits-all)
- Teacher report: top students needing attention with reasons and actions taken
- Dashboard charts: risk curves, actions executed, score distribution with vs. without the agent
- Self-adjusting risk threshold (Learn step)
- Safe `simulation_mode` switch

## 📊 Sample results (synthetic data, seed 42)

| Metric | Value |
|--------|-------|
| Fail-risk ROC-AUC (unseen students) | ~0.95 |
| Final-score prediction error (MAE) | ~5 marks |
| Failing students caught at week 6 | 26 of 30 (precision ~87%) |
| Students failing without agent → with agent | 30 → 17 |

The "with agent" improvement depends on the **assumed effect of each intervention** (`intervention_effect` in the config). It is a simulation assumption, not real-world evidence. Results on real data will differ.

## 🛠️ Tech stack

Python 3.9+ · pandas · NumPy · scikit-learn · Matplotlib · Jupyter Notebook

## ▶️ Getting started

```bash
# 1. Clone the repo
git clone https://github.com/<your-username>/NexaPulse.git
cd NexaPulse

# 2. (Optional) create a virtual environment
python -m venv venv
source venv/bin/activate        # Windows: venv\Scripts\activate

# 3. Install dependencies
pip install -r requirements.txt

# 4. Launch Jupyter and open the notebook
jupyter notebook NexaPulse_Student_Agent.ipynb
```

In Jupyter, choose **Kernel → Restart & Run All**. No dataset download is needed.

## ⚙️ Configuration

Edit the `CONFIG` dictionary in the first code cell:

| Key | Meaning |
|-----|---------|
| `n_past` / `n_live` | Number of students in the training / current semester |
| `weeks`, `start_week` | Semester length and the week the agent starts predicting |
| `pass_mark` | Final score below this counts as a fail |
| `risk_threshold` | Risk level at which the agent acts |
| `cooldown_weeks` | Minimum gap before repeating the same action for a student |
| `simulation_mode` | `True` = actions only simulated |
| `intervention_effect` | Simulated score boost per action (assumption) |

## 📁 Project structure

```
NexaPulse/
├── NexaPulse_Student_Agent.ipynb   # Main notebook (data, model, agent, dashboard)
├── requirements.txt                # Python dependencies
└── README.md
```

## 🔌 Using real data

1. Replace `generate_students()` with a loader for your CSV / Excel / database. Keep the columns: `student_id, week, attendance, study_hours, quiz, assignment, late_count`, plus a final score per student for training.
2. Replace the simulated messages in `act()` with real email (`smtplib`), SMS or WhatsApp API calls, and set `simulation_mode = False`.

## ⚖️ Ethics & limitations

- Student data is sensitive: follow your institution's privacy rules and get consent.
- Models can be biased; check fairness before real use.
- The agent only **suggests and supports**. Final decisions should always stay with teachers and mentors.
- Synthetic data is cleaner than real data, so the accuracy shown here is optimistic.

## 🗺️ Roadmap

- [ ] Streamlit dashboard for teachers
- [ ] SHAP explanations for each prediction
- [ ] XGBoost / LSTM models
- [ ] Real email / WhatsApp integration
- [ ] LLM-based planner for personalised messages

## 👤 Author

**Mukesh P**
GitHub: [25mca032-svg](https://github.com/25mca032-svg) · LinkedIn: [mukesh-p](https://www.linkedin.com/in/mukesh-p-9274b5259)

---

⭐ If you find this project useful, consider giving it a star!
