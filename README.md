# sox-rec-pipe-to-ffmpeg-split-files
How to record input audio stream with sox rec, pipe it to ffmpeg and split resulting outfiles

AUDIODRIVER=alsa rec -b 16 -r 44100 -t wav - silence 1 0.0 0% 1 5.0 3%| ffmpeg -f wav -i pipe: -f segment -segment_time 10 -c copy /share/test%02d.wav

Translates to:
AUDIODRIVER=alsa tells rec which driver to use in case more drivers are available on the system
