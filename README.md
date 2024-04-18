# Pm118..
import numpy as np
from scipy.signal import correlate, convolve

# Function to generate a random signal
def generate_signal(length):
    """
    Generate a random signal of specified length.
    """
    return np.random.rand(length)

# Function to save a signal to a text file
def save_signal(signal, filename):
    """
    Save the signal to a text file.
    """
    with open(filename, 'w') as file:
        for value in signal:
            file.write(f"{value}\n")

# Function to load a signal from a text file
def load_signal(filename):
    """
    Load the signal from a text file.
    """
    with open(filename, 'r') as file:
        signal = np.array([float(line.strip()) for line in file])
    return signal

# Generate input and output signals
input_signal = generate_signal(100)  # Example: Generate a random input signal of length 100
output_signal = generate_signal(100)  # Example: Generate a random output signal of length 100

# Save input and output signals to text files
save_signal(input_signal, "input_signal.txt")
save_signal(output_signal, "output_signal.txt")

# Define filter kernels (hlp, hhp, hbp)
hlp = np.array([1, 1, 1]) / 3  # Example: Low-pass filter kernel
hhp = np.array([-1, 2, -1])  # Example: High-pass filter kernel
hbp = np.array([-1, 2, -1, 2, -1])  # Example: Band-pass filter kernel

# Convolve filters with input signal to get filtered outputs
lp_filtered_signal = convolve(input_signal, hlp, mode='same')
hp_filtered_signal = convolve(input_signal, hhp, mode='same')
bp_filtered_signal = convolve(input_signal, hbp, mode='same')

# Compute correlations between filtered outputs and output signal
lp_correlation = correlate(lp_filtered_signal, output_signal, mode='same')
hp_correlation = correlate(hp_filtered_signal, output_signal, mode='same')
bp_correlation = correlate(bp_filtered_signal, output_signal, mode='same')

# Identify the filter that best matches the output signal
best_filter = None
if np.max(lp_correlation) > np.max(hp_correlation) and np.max(lp_correlation) > np.max(bp_correlation):
    best_filter = "Low Pass"
elif np.max(hp_correlation) > np.max(lp_correlation) and np.max(hp_correlation) > np.max(bp_correlation):
    best_filter = "High Pass"
else:
    best_filter = "Band Pass"

# Save the filtered signals and analysis results
save_signal(lp_filtered_signal, "lp_filtered_signal.txt")
save_signal(hp_filtered_signal, "hp_filtered_signal.txt")
save_signal(bp_filtered_signal, "bp_filtered_signal.txt")

# Save the analysis results
with open("analysis_results.txt", "w") as file:
    file.write(f"Best matching filter: {best_filter}\n")
    file.write("Correlation results:\n")
    file.write(f"Low Pass: {np.max(lp_correlation)}\n")
    file.write(f"High Pass: {np.max(hp_correlation)}\n")
    file.write(f"Band Pass: {np.max(bp_correlation)}\n")
    
