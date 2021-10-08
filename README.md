# sox-rec-pipe-to-ffmpeg-split-files
How to record input audio stream with sox rec, pipe it to ffmpeg and split resulting outfiles

AUDIODRIVER=alsa rec -q -b 16 -r 44100 -t wav - silence 1 0.0 0% 1 20.0 3%| ffmpeg -f wav -i pipe: -f segment -segment_time 3600 -c copy /share/test%02d.wav

Translates to:\
AUDIODRIVER=alsa --> tells rec which driver to use in case more drivers are available on the system\
\
rec .... --> tell it to shut up (-q; we check recording levels beforehand carefully and now only want to see the screen output of ffmpeg), specify bitrate, resolution and type of the "file" (we pipe out a stream!) to be recorded\
NOTE the " - " after "-t wav" THIS tells rec to pipe the stream outward\
\
silence ... --> START recording immediately (0.0 seconds after level is over 0%) and STOP recording once level falls 20.0 seconds under 3%\
\
At the receiving end of the pipe we tell ffmpeg to write out a wav file, take input from pipe and write in segments of 1 hour to a 2-digit-test.wav file (on a smb share or easy access later on)\
\
This should come in handy for long recording sessions that ends automagically once level drops and have "not too long" output files for post-processing.\
\
post-pro could look like:\
We start a new ssh session and on the remote we "screen -S my_recording_session", fire the rec command, DETACH the session via CTRL+ a d and close the ssh session. After recording is finished we grab the files off the share and do post processing (cut silence at beginning, normalise to -2 db, mark noise in audacity, export multiple to .mp3).

