# 🛡️ Enterprise Oracle 19c CPU Patching Automation Engine

A robust, configuration-driven Bash automation framework designed by Senior DBAs to safely streamline quarterly Critical Patch Updates (RU, OJVM, and One-Off patches) in Linux-based Oracle 19c environments.

This engine completely eliminates human error during complex maintenance windows, offering a fail-fast architecture, parallel infrastructure execution, and container-ready validation.

---

## ⚡ Key Architecture & Features

* **Unified Lifecycle Orchestration:** Seamlessly manages environment pre-checks, listener and multi-database graceful shutdown, binary patching (`OPatch`), database `datapatch`, and automated object recompilation (`utlrp`).
* **Multitenant (CDB/PDB) Aware:** Automatically ensures invalid objects across all pluggable containers are accurately compiled and state-tracked.
* **Differential Object Verification:** Captures and compares detailed database invalid object counts *before* and *after* the maintenance window, generating a clean differential summary.
* **Flexible Execution Topology:**
  * **Serial Mode:** Executes the full pipeline in a single automated session.
  * **Parallel/Distributed Mode:** Splits binary execution (`--phase home`) and database-level execution (`--phase db`), giving infrastructure teams maximum control over rolling windows.
* **Fail-Fast Shell Engineering:** Built with strict `set -euo pipefail` safeguards. If a database or database listener gets stuck, the engine aborts safely to prevent registry corruption.
* **Simulation Mode (`-s / --dry-run`):** Validates inventory availability, variable structures, and patch staging trees without modifying production environments.

---

## 📋 Execution Options & Staging

The package is fully generic, cyclical, and driven by a simple centralized configuration layout:

```bash
# 1. Simulate/Dry-Run your entire quarterly environment setup
./oracle_cpu_patch.sh -c patch.conf -s

# 2. Run the full unified patching pipeline
./oracle_cpu_patch.sh -c patch.conf

# 3. Distributed Execution (Advanced Operations)
./oracle_cpu_patch.sh -c patch.conf --phase home      # Binary only
./oracle_cpu_patch.sh -c patch.conf --phase db        # Datapatch & Recompilation
