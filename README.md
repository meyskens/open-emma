# Open Source EMMA

EMMA (Electronic Management of Message and Announcement) is the announcement system of NMBS/SNCB and Infrabel (ownership changed over time).
This projet aims to be an open library of audio messages by EMMA, for use by train fans.

## More about EMMA
* [Video Infrabel NL](https://www.youtube.com/watch?v=MFhZfuNl2iM)
* [Video SNCB Inside](https://www.youtube.com/watch?v=qnrqK25dHvM)

## How this works
Since no official recordings are available we rely on recorings inside train stations.
These have a lot of background noise, this we can filter out with the help of a Neural Net.
We use [Looking to Listen](https://github.com/meokz/looking-to-listen), we use a modification of the Google Research to do an audtio only analysis.
Cleaned up recordings are placed in `cleaned/`, these are then cut into pieces with the best recording of each word.

### How to clean up recordings
You will need a sound recording of EMMA in WAV format.
You will also need a copy of https://github.com/meokz/looking-to-listen. Note: this uses a lot of RAM, running it in a cloud instance is adviced.

The neural net has issues processing silence, this can be filtered using ffmpeg:
```
 ffmpeg -i IN.wav -af "silenceremove=start_periods=1:start_duration=1:start_threshold=-60dB:detection=peak,aformat=dblp,areverse,silenceremove=start_periods=1:start_duration=1:start_threshold=-60dB:detection=peak,aformat=dblp,areverse" OUT.wav
```
After that you can place it inside `./data/noice/` of the looking-to-listen directory and launch it using 
```
docker-compose run network python3 quick_start_audio_only.py /data/model/0f_1sclean_noise.npz /data/noise -ideep
```
The cleaned up can be found in `./data/result/name/clean.wav`.

## Project structure
`cleaned/` contains long form audio samples which are cleaned up by the neural net.
`en/ nl/ fr/ de/` are the languages folders.
In each of these are sub-categories:
* `numbers` for numbers
* `trains` for train types eg. IC, IR, L, S1, Thalys, Eurostar
* `stations` contains station names in telegegraph notation (see http://users.telenet.be/pk/stations.htm) in case of doubt (Noorderkempen) use the NMBS notation

(structure is work in progress)

for versioning of old recordings we should trust Git and not rely on versioning in file names.

