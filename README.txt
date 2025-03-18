Youtube Scraper and Preprocessor

This project automates the scraping, downloading, and preprocessing of YouTube videos
using Python scripts and external tools like yt-dlp, OpenCV, Apple's Vision API, and ffmpeg.
It gathers video URLs via automated searches, downloads video segments with GPU acceleration
optimized with Apple Silicon in this case M3 Max focus and validates face visibility, trims
videos to specific durations, removes duplicates, and maintains organized metadata logs.

Functionality: 

URL Gathering: Uses SERPAPI to perform automated YouTube searches based on customizable
queries, retries the URLs while optionally filtering for Creative Commons Licenses and
accumulates unique URLs into 'video_urls.txt,' avoiding duplicates.

Video Download and Processing: Downloads videos from YouTube using yt-dlp. I've made
it to where I use GPU-accelerated video encoding with ffmpeg via apple's H264 videobox
for optimizing usage on Apple Silicon. It also enables for face visibility validation on
videos using Apple's GPU-accelerated vision API for fast and efficient face detection.
For cleaning to our case usage I've made all videos be validated and trimmmed to specific 
durations, and log each detailed video metadata into 'video_metadata.csv' in a thread-safe
manner.

Batch Processing and Deduplication: Concurrently processes multiple videos to maximize
resource efficiency. Removes duplicate video files based on file hashing, ensuring storage
optimization and elimination of redundencies to allow for repeated runs. I've also
allowed for real time logging and detaled error management into the code during the batching.

STRUCTURE:

cscraper/
├── scripts/
│   ├── url_gather.py       # Searches YouTube and gathers video URLs
│   ├── scraper.py          # Downloads, validates, and processes videos
│   └── preprocess.py       # Batch processing script with concurrency and duplicate removal
├── downloads/
│   └── youtube/
│       ├── raw/            # Directory for raw downloaded videos
│       └── video_metadata.csv # Metadata for downloaded videos
├── video_urls.txt          # List of YouTube URLs
└── .venv/                  # Virtual Python environment

SETUP:

Prerequesuites: Install Homebrew

1. Install Python 3.12:
brew install python@3.12

2. Set up Python Virtual Environment:
python3.12 -m venv .venv
source .venv/bin/activate

3.	Install Python Dependencies:
pip install requests moviepy==1.0.3 imageio imagio-ffmpeg opencv-python face_recognition

4. Install external tools
brew install yt-dlp ffmpeg	

Use:

Make sure all the urls are there by customizing your request for video pulling with the
use of SERPAPI and your key. You run it and all your URLS will be put into video_urls.txt
keep in mind each keyword frame will be n amount of request specified so if your cap is 20
and you have n keyterm combinations expect n(20) URLs.

Also keep in mind for research purposes you may disable and enable FILTER_CREATIVE_COMMONS
by toggling True or False as seen in the code. The same applies for aggregate running of the
program by toggling on and off of CLEAR_CSV in preprocess.py for metadata logging.

Commands:
source .venv/bin/activate
python scripts/url_gather.py
python scripts/preprocess.py


Problems:
Having a Doom Loop issue with dtl failing somehow
