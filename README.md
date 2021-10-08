# sox-rec-pipe-to-ffmpeg-split-files
How to record input audio stream with sox rec, pipe it to ffmpeg and split resulting outfiles

AUDIODRIVER=alsa rec -b 16 -r 44100 -t wav - silence 1 0.0 0% 1 20.0 3%| ffmpeg -f wav -i pipe: -f segment -segment_time 3600 -c copy /share/test%02d.wav

Translates to:\
AUDIODRIVER=alsa --> tells rec which driver to use in case more drivers are available on the system\
rec .... --> specify bitrate, resolution and type of the "file" (we pipe out a stream!) to be recorded\
NOTE the " - " after "-t wav" THIS tells rec to pipe the stream outward\
silence ... --> START recording immediately (0.0 seconds after level is over 0%) and STOP recording once level falls 20.0 seconds under 3%\
At the receiving end of the pipe we tell ffmpeg to write out a wav file, take input from pipe and write in segments of 1 hour to a 2-digit-test.wav file\
This should come in handy for long recording session that ends automagically once level drops and have "not too long" output files for post-processing.

