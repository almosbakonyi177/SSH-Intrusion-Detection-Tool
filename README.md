# SSH-log-analyzer
Python tool that analyzes SSH authentication logs and flags likely brute-force attacks and targeted accounts. It combines a sliding-window burst detector with a percentile-based anomaly check, and runs offline on log files.
## Features
- **Burst detection (sliding window)**: flags an IP with 5 or more failed logins within any 1-hour window.
- **Statistical anomaly detection**: uses NumPy to calculate the 95th percentile of failed login counts per day, surfacing high-volume attack vectors.
- **Targeted usernames**: lists accounts with 5 or more failed logins in a day.
- **IP allowlist**: trusted IPs from a text file are ignored in every check.
- **Tkinter GUI**: pick a log file and an allowlist, set the anomaly threshold, and view results.

## How it works
1. Parses each Failed password / Accepted password line with regex to extract date, time, username and IP (IPv4 and IPv6).
2. Groups attempts by day.
3. For each IP, sorts failure timestamps and moves a two-pointer window over them: O(n log n) per IP.
4. Computes the 95th percentile of failures per IP with NumPy and reports outliers.
5. Counts failed logins per username and reports accounts with 5 or more.


## Limitations
- Analyzes saved log files; it does not monitor logs in real time.
- Detection thresholds (5 failures / 1 hour) are fixed in the code.
