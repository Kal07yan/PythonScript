![PythonScript2](https://github.com/Kal07yan/PythonScript/assets/104569755/c42c54d2-da25-45ee-a597-5a445984593e)
![PythonScript1](https://github.com/Kal07yan/PythonScript/assets/104569755/16e60864-2ff3-4d76-a13c-014e60b02bb9)
# PythonScript
import subprocess
import signal
import sys
import collections

# Define the log file path
LOG_FILE_PATH = "/path/to/your/logfile.log"

# Keywords for log analysis
KEYWORDS = ["error", "warning", "HTTP"]

# Function to continuously monitor log file
def monitor_log_file():
    try:
        # Open the log file in subprocess to continuously monitor new entries
        tail_process = subprocess.Popen(['tail', '-F', LOG_FILE_PATH], stdout=subprocess.PIPE, stderr=subprocess.PIPE)

        # Initialize keyword counts
        keyword_counts = {keyword: 0 for keyword in KEYWORDS}

        # Continuously read new log entries
        for line in iter(tail_process.stdout.readline, b''):
            line = line.strip().decode('utf-8')  # Decode log entry
            print(line)  # Display log entry

            # Perform log analysis
            for keyword in KEYWORDS:
                if keyword in line:
                    keyword_counts[keyword] += 1

            # Print summary report
            print_summary_report(keyword_counts)

    except KeyboardInterrupt:
        print("Monitoring stopped by user.")
        tail_process.terminate()  # Terminate tail process
        sys.exit(0)
    except Exception as e:
        print(f"An error occurred: {e}")
        sys.exit(1)

# Function to print summary report
def print_summary_report(keyword_counts):
    print("\nSummary Report:")
    print("================")
    for keyword, count in keyword_counts.items():
        print(f"{keyword}: {count}")

def signal_handler(signal, frame):
    print("\nMonitoring stopped by user.")
    sys.exit(0)

if __name__ == "__main__":
    # Register signal handler for Ctrl+C
    signal.signal(signal.SIGINT, signal_handler)

    # Continuous log monitoring loop
    print("Starting log monitoring...")
    monitor_log_file()






**Explanation**
This Python script continuously monitors a specified log file for new entries, performs basic log analysis, and generates summary reports. It uses the tail command to track and display new log entries in real-time and implements a mechanism to stop the monitoring loop using Ctrl+C.

**Prerequisites**
Python 3.x
Linux or macOS system (for tail command support).

**Usage**
Clone the repository or download the script file (log_monitor.py).
Make sure the log file you want to monitor exists and is accessible.
Open a terminal and navigate to the directory containing the log_monitor.py script.
Run the script using the following command:  **python log_monitor.py**
The script will start monitoring the specified log file and display new log entries in real-time. Press Ctrl+C to stop the monitoring process.

**Log Analysis**
The script performs basic log analysis by counting occurrences of specific keywords or patterns in the log entries.
Customize the KEYWORDS list in the script to specify the keywords or patterns you want to analyze.
                            
1.monitor_log_file() function continuously monitors the specified log file using the tail -F command through subprocess. It reads and prints new log entries in real-time.
2.When the user interrupts the process (e.g., using Ctrl+C), the KeyboardInterrupt exception is caught, and the monitoring loop is stopped gracefully by terminating the subprocess.
3.signal_handler() function handles the Ctrl+C signal and exits the script gracefully.
4.Make the script executable using the command chmod +x log_monitor.sh.
5.Run the script using ./log_monitor.sh.
6.This script continuously displays new log entries from the specified log file in real-time. Pressing Ctrl+C stops the monitoring loop.
