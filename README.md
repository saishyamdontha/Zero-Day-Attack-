Overview
This project implements a production-grade dual-layer intrusion detection system that identifies zero-day attacks by learning exclusively from benign network traffic and system activity. The system combines:

Network Layer: CIC-IDS 2017 dataset analysis using association rule mining on sliding windows

Host Layer: ADFA-LD system call trace analysis using behavioral pattern discovery

Fusion Layer: OR-logic correlation of independent detection signals

Key Innovation: Unsupervised learning from normal behavior only—no labeled attack data required during training.

Performance: Achieves 92.6% recall on known attacks and 54.3% on unseen zero-day attack families.

Repository Structure

zero_day_intrusion_detection/
├── analysis_pipeline.ipynb              # Complete end-to-end analysis
├── datasets/
│   ├── cicids_2017/                     # Network traffic (CIC-IDS)
│   │   ├── monday.csv                   # Benign training data
│   │   ├── tuesday.csv                  # Test data (attacks)
│   │   ├── wednesday.csv
│   │   └── thursday.csv
│   └── adfa-ld/                         # Host system calls (ADFA-LD)
│       ├── Training_Data_Master/        # Normal activity
│       └── Attack_Data_Master/          # Attack traces
├── src/                                 # Processing modules
│   ├── sliding_window.py
│   ├── discretize.py
│   ├── rule_mining.py
│   └── correlation.py
├── outputs/                             # Generated results
│   ├── detection_results.csv
│   ├── performance_summary.txt
│   └── anomaly_scores.pkl
├── requirements.txt                     # Dependencies
└── README.md                            # You're reading it!
System Architecture

┌─────────────────┐    ┌──────────────────┐    ┌─────────────────┐
│   Network Layer │    │   Host Layer     │    │  Correlation    │
│  (CIC-IDS2017)  │    │   (ADFA-LD)      │    │     Layer       │
│                 │    │                  │    │                 │
│  1. Windowing   │───▶│  1. N-gram       │───▶│  OR-Logic       │
│  2. Discretize  │    │  2. Rule Mining  │    │  Fusion         │
│  3. Rule Mining │    │  3. Anomaly      │    │                 │
│  4. Anomaly     │    │     Scoring      │    │  Alert!         │
└─────────────────┘    └──────────────────┘    └─────────────────┘
Quick Start
Prerequisites
Python 3.8+

~2GB RAM available

5GB disk space for datasets

Installation (2 minutes)
bash
 Clone repository
git clone <https://github.com/itssaideep/zero_day_intrusion_detection>
cd zero_day_intrusion_detection

 Install dependencies
pip install -r requirements.txt

 Launch Jupyter
jupyter notebook analysis_pipeline.ipynb
Dataset Download
The notebook includes automatic download links, or manually place datasets in datasets/ following the structure above.

Usage Instructions
Step-by-Step Execution
Open analysis_pipeline.ipynb

Update paths in Configuration section (if needed)

Run all cells sequentially (recommended first run)

Review outputs in outputs/ folder

Tune parameters in Configuration section for your environment


Cell 1-3: Configuration & Data Loading
Cell 4-10: Network Layer Processing  
Cell 11-13: Host Layer Processing
Cell 14-16: Correlation & Results
Cell 17+: Visualizations & Summary

Expected Runtime
Component	    Duration	Output Files
Network Layer	~15 min	    step6_anomaly_results.csv
Host Layer	    ~8 min	    adfa_results.pkl
Correlation	    ~2 min	    final_results.csv
Total	        ~25 min	    All outputs

Configuration Parameters

Network Layer
Parameter	    Default	    Purpose
window_size	    20	        Flows per analysis window
window_step	    10	        Overlap between windows
alert_threshold	0.10	    Anomaly detection cutoff
max_rules	    50	        Retained behavioral patterns

Host Layer
Parameter	   Default	Purpose
host_threshold	0.45	   System call anomaly cutoff
ngram_length	3-5	   Sequence length analyzed

Fusion Logic

ALERT = (network_score ≥ 0.10) OR (host_score ≥ 0.45)

Key Results

Layer	    Recall	   Precision	F1-Score	   Zero-Day Recall
Network	 79.05%	   94.33%	    0.861	    -
Host	    64.88%	   83.45%	    0.730	    54.3%
Combined	 92.6%	   89.2%	       0.908	    54.3%
Notable Coverage Gains:

Web Brute Force: +72.08pp (4.08% → 76.16%)

Web XSS: +55.68pp (10.00% → 65.68%)

Output Files Explained
File	Description	Usage
detection_results.csv	Per-attack detection metrics	Performance analysis
performance_summary.txt	Executive overview	Quick status check
anomaly_scores.pkl	Raw detection confidence	Custom analysis
feature_correlation.png	Network feature relationships	Model interpretability
Technical Methodology
Core Innovation: Association Rule Mining

1. Learn normal patterns from benign data only:
   {HIGH_flow_rate, LOW_packet_size, port_80} → normal (conf: 0.92)

2. Score new traffic against normal rules:
   anomaly_score = violated_weight / applicable_weight

3. Alert on significant deviations (threshold > 0.10)
Rule Quality Metrics

Minimum Support:    ≥ 0.05 (5% of transactions)
Minimum Confidence: ≥ 0.82 (82% reliability)  
Minimum Lift:       ≥ 1.8 (1.8x more likely than random)
Production Deployment

System Requirements

• Real-time flow ingestion (10K flows/sec)
• 4 vCPU, 16GB RAM per monitoring node
• Redis for rule caching
• PostgreSQL for alert storage
Scalability Features
Stateless rule evaluation

Horizontal pod autoscaling

Rolling model updates

Monitoring Dashboard Metrics

• Current anomaly score (0.0-1.0)
• Rule drift percentage
• Detection latency (ms)
• False positive rate (hourly)

Validation Datasets

CIC-IDS 2017 (Network)

Training: 371,621 benign flows (Monday)
Testing:  Mixed benign + 20 attack types (Tue-Fri)
Attacks: DDoS, PortScan, Brute Force, Web Attacks
ADFA-LD (Linux Host)

Training: 833 normal process traces
Testing: 6 attack families (3,410 traces)
Attacks: Meterpreter, Web Shells, Privilege Escalation
Limitations & Future Work
Current Constraints
Dataset Age: 2017 data (modern encrypted attacks untested)

Lab Environment: Production traffic dynamics unmodeled

Static Thresholds: Manual tuning required per environment


Contributing
Fork repository

Create feature branch (git checkout -b feature/amazing-feature)

Commit changes (git commit -m 'Add amazing feature')

Push to branch (git push origin feature/amazing-feature)

Open Pull Request

License
This project is licensed under the MIT License - see the LICENSE file for details.
# Zero-Day-Attack-
