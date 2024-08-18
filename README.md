About :
This script enables you to select the video and extend it to given number of hours 



```python

from moviepy.editor import VideoFileClip, concatenate_videoclips
import math
from tkinter import Tk
from tkinter.filedialog import askopenfilename, asksaveasfilename

def loop_video_optimized(input_file, output_file, hours):
    # Load the video clip
    clip = VideoFileClip(input_file)
    
    # Calculate the total duration needed
    total_duration = hours * 3600  # in seconds
    
    # Create a long enough chunk to reduce the number of concatenations
    chunk_duration = min(total_duration, 3600)  # Create a 1-hour chunk
    chunk_loops = math.ceil(chunk_duration / clip.duration)
    chunk = concatenate_videoclips([clip] * chunk_loops)
    
    # Calculate the number of full chunks required
    num_chunks = math.floor(total_duration / chunk.duration)
    
    # If needed, calculate the duration of the remaining part
    remainder_duration = total_duration - num_chunks * chunk.duration
    
    # Concatenate the full chunks
    final_chunks = [chunk] * num_chunks
    
    # Add the remaining part if necessary
    if remainder_duration > 0:
        final_chunks.append(chunk.subclip(0, remainder_duration))
    
    # Concatenate all chunks together
    final_video = concatenate_videoclips(final_chunks)
    
    # Write the result to a file
    final_video.write_videofile(output_file, codec="libx264", preset="fast", threads=4, audio_codec="aac")

# Hide the root window
Tk().withdraw()

# Ask the user to browse for the input video file
input_video_path = askopenfilename(title="Select the input video file", filetypes=[("Video files", "*.mp4 *.avi *.mov *.mkv")])

# Ask the user to browse for the output directory and specify the file name
output_video_path = asksaveasfilename(title="Save the output video as", defaultextension=".mp4", filetypes=[("MP4 files", "*.mp4")])

# Ask the user for the desired duration in hours
hours = float(input("Enter the desired duration in hours: "))

# Call the function with user inputs
loop_video_optimized(input_video_path, output_video_path, hours)


```

![image](https://github.com/user-attachments/assets/0eec60b7-944c-4e6d-a6ee-7475ce6a7c42)

