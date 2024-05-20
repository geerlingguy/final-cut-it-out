# Final Cut It Out

An automatic silence remover for Final Cut Pro X.

## Origins

This project has its origins in [jashmenn's final-cut-it-out.js script](https://gist.github.com/jashmenn/66f2806ae6da643a0bb16452629deee8).

All credit goes to him and those who have left follow-up comments on his Gist.

## Prerequisites

This script is meant to work with Final Cut Pro X (on a Mac), and requires the use of `ffmpeg` to detect silent portions of the clip.

You can install Final Cut Pro X from the app store, and `ffmpeg` using Homebrew: `brew install ffmpeg`.

You should clone or download this project to somewhere on your computer, and in the Terminal, change directories into this project's directory.

## Usage

The following is how _I_ use this script. You could have a simpler or more complex workflow, depending on your needs.

  1. Insert the video clip with silent portions into an open project timeline in Final Cut Pro X.
  1. Export the video to a low-resolution file that can be processed via `ffmpeg`.
  1. Run the following `ffmpeg` command to detect silence in your video: `ffmpeg -i [VIDEO.MP4] -af silencedetect=n=-35dB:d=800ms -f s16le -y /dev/null 2>&1 | tee silence.txt`
  1. Run `./final-cut-it-out.js silence.txt` and watch as the script makes cuts through the timeline at the margins of each silent section, then goes back and deletes all the silent portions.
  1. Run through the timeline and adjust the gaps using the select/trim tool as desired.

## Configuration

Before running this script, you should always have a backup Project/timeline in FCPX since this will make destructive edits to your video timeline! Cmd-Z should not be relied on in case of disaster—you've been warned!

I currently record at 23.98p, so some of the settings within `moveToTimecode()` have been changed accordingly. The original script was written with 60p footage in mind.

Someday it would be nice to modify the script to account for different frame rates... but for now, you'll need to modify the script if you shoot at 30p or 60p.

The other parameters you may wish to tweak, based on your own speaking style and the rhythm of your speech:

  - In the `ffmpeg` command, `silencedetect=n=-35dB:d=800ms`: change the dB setting to a higher or lower threshold if you have softer or louder audio and are getting parts cut out that you do not want cut. Adjust the `d` value (how long a portion is silent before it is marked for deletion) as needed.
  - In the script, adjust the `startMargin` and `endMargin` to your liking. I like a little bit of space before and after the audio, but some people like a bit more of a 'jump' cut style so should reduce these values a bit.
