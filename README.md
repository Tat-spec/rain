# rainfrom pydub import AudioSegment
from pydub.playback import play
import numpy as np

def generate_rain_sound(duration_ms=5000):
    sample_rate = 44100
    
    # Generate white noise
    noise = np.random.normal(0, 1, int(sample_rate * duration_ms / 1000))
    
    # Apply a low-pass filter to make the noise sound more like rain
    freq = 1000  # Frequency cutoff for the low-pass filter
    noise = np.fft.rfft(noise)
    freqs = np.fft.rfftfreq(len(noise))
    noise = noise * (freqs < freq)
    noise = np.fft.irfft(noise)
    
    # Convert to 16-bit audio format
    audio = np.int16(noise * 32767)
    
    # Create an audio segment from the generated noise
    rain_sound = AudioSegment(audio.tobytes(), frame_rate=sample_rate, sample_width=2, channels=1)
    
    return rain_sound

# Generate the rain sound
rain_sound = generate_rain_sound()

# Play the rain sound
play(rain_sound)
