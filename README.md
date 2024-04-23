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
