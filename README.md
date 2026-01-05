# KdumpAgent
KdumpAgent is an AI-powered agent that autonomously performs root cause analysis on Linux kernel crash dumps using an agentic workflow.
It is an **Agentic AI** tool designed to act like a Senior Linux Kernel Engineer. , interacting with the `crash` utility to diagnose system failures and generating a professional HTML report with its findings. Unlike traditional tools that perform a "one-shot" analysis of a static log file, KdumpAgent uses an **interactive "Observe-Think-Act" workflow**. It connects directly to the Linux `crash` utility, iteratively executes commands, interprets the output, and decides the next logical step to drill down into the root cause of a kernel panic.

---

### ✨ Key Features
*   ** Agentic Workflow:** Implements a dynamic loop (Observe -> Think -> Act) to investigate issues step-by-step.
*   ** Interactive Investigation:** Autonomously executes `crash` commands (e.g., `sys`, `bt`, `kmem`, `ps`) based on real-time findings.
*   ** Chain-of-Thought Reasoning:** Analyzing context, identifying anomalies (OOM, Deadlocks, Null Pointers), and formulating hypotheses.
*   ** Structured Reporting:** Generates a professional HTML dashboard containing the Conclusion, Technical Root Cause, Evidence, and Fix Recommendations.
*   ** Audit Trail:** Automatically logs every command executed and the reasoning behind it for full transparency.

---

## Why KdumpAgent?

| Feature | Traditional AI Analysis | **KdumpAgent** |
| :--- | :--- | :--- |
| **Input** | Static text (logs/backtrace) | Dynamic interaction with `vmcore` |
| **Process** | Single-pass prediction | **Multi-turn investigation loop** |
| **Context** | Limited to what is pasted | **Unlimited** (can query memory, tasks, registers) |
| **Accuracy** | Prone to hallucinations based on limited data | **Verifies hypotheses** with actual commands |


### ⚙️ How It Works

KdumpAgent uses a methodical, closed-loop process to analyze a crash dump. At each step, it:

1.  **OBSERVE**: It examines the output of previous `crash` commands (like `bt`, `log`, `kmem`).
2.  **THINK**: Based on the data, it forms a hypothesis. For example, "The backtrace points to a null pointer dereference in module X. I need to verify the registers."
3.  **ACT**: It executes new, more specific `crash` commands to test its hypothesis.

This loop continues until the agent is confident it has found the root cause, at which point it generates a final report.

```+---------------------------+
|      KdumpAgent Loop      |
+---------------------------+
|                           |
|   [ 1. OBSERVE ]          |
|   Read crash output       |
|       +-----------------> |
|                           |
|   [ 2. THINK ]            |
|   Formulate Hypothesis    |
|       +-----------------> |
|                           |
|   [ 3. ACT ]              |
|   Execute new commands    |
|       +-----------------> |
|                           |
|   Is RCA found? ---> YES ---> [ Finish & Report ]
|       |
|       NO
|       |
+-------+-------------------+
```

---

### 🚀 Getting Started

#### Prerequisites

1.  **Linux Environment**: A system with Python 3.6+ installed.
2.  **`crash` Utility**: The `crash` analysis tool must be installed.
    -   On RHEL/CentOS/Fedora: `sudo dnf install crash`
    -   On Debian/Ubuntu: `sudo apt-get install crash`
3.  **Debug Symbols**: The corresponding `vmlinux` file with debug information for the kernel that crashed.

#### Installation

1.  **Clone the repository:**
    ```bash
    git clone https://github.com/your-username/KdumpAgent.git
    cd KdumpAgent
    ```

#### Configuration

Open the `KdumpAgent.py` script and edit the configuration section at the top:

```python
# --- Configuration ---
# IMPORTANT: Replace with your actual API Key and endpoint
AI_API_KEY = "your-secret-api-key-goes-here"
AI_API_URL = "https://your.llm.provider/api/v3/chat/completions"
MODEL_NAME = "deepseek-r1-250528" # Or any other powerful model
```
---

### 💻 Usage

Execute the script from your terminal, providing the paths to the `vmcore` and `vmlinux` files.

```bash
python3 KdumpAgent.py --vmcore <path_to_vmcore> --vmlinux <path_to_vmlinux> [--output my_report.html]
```

**Example:**

```bash
python3 KdumpAgent.py \
  --vmcore /var/crash/127.0.0.1-2026-01-05-10:30:00/vmcore \
  --vmlinux /usr/lib/debug/lib/modules/5.14.0-284.el9_2.x86_64/vmlinux \
  --output /root/crash_report_for_server_A.html
```

The agent will start its analysis loop, printing its thought process at each step. Once finished, a detailed HTML report will be saved to the specified output file (or `kdump_agent_report.html` by default).

<img width="3010" height="2016" alt="image" src="https://github.com/user-attachments/assets/fb259525-8550-408b-a66a-0792be05daeb" />


### 📜 Sample Report

The generated report is a self-contained HTML file that looks like this, it shows the header, severity, conclusion, root cause, and audit log sections.

<img width="2258" height="1888" alt="image" src="https://github.com/user-attachments/assets/96ae4294-21ea-4c5b-8e40-b70695e900a5" />

## Support Contact

For issues or questions, contact:

Frank Zhu [flankeroot@gmail.com](mailto:flankeroot@gmail.com)  




