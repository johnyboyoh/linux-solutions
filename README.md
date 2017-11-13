# linux-solutions
here I write every solution that took me more than 5 minutes to reach on linux

## download from google drive using bash

solution is from here:
https://stackoverflow.com/questions/25010369/wget-curl-large-file-from-google-drive
.Tested Oct 2017. 

```python
import requests

def download_file_from_google_drive(id, destination):
    def get_confirm_token(response):
        for key, value in response.cookies.items():
            if key.startswith('download_warning'):
                return value

        return None

    def save_response_content(response, destination):
        CHUNK_SIZE = 32768

        with open(destination, "wb") as f:
            for chunk in response.iter_content(CHUNK_SIZE):
                if chunk: # filter out keep-alive new chunks
                    f.write(chunk)

    URL = "https://docs.google.com/uc?export=download"

    session = requests.Session()

    response = session.get(URL, params = { 'id' : id }, stream = True)
    token = get_confirm_token(response)

    if token:
        params = { 'id' : id, 'confirm' : token }
        response = session.get(URL, params = params, stream = True)

    save_response_content(response, destination)    


if __name__ == "__main__":
    import sys
    if len(sys.argv) is not 3:
        print "Usage: python google_drive.py drive_file_id destination_file_path"
    else:
        # TAKE ID FROM SHAREABLE LINK
        file_id = sys.argv[1]
        # DESTINATION FILE ON YOUR DISK
        destination = sys.argv[2]
        download_file_from_google_drive(file_id, destination)

```
## install open AI gym on windows:

based on: 
https://github.com/openai/gym/issues/11

pip install gym
pip install git+https://github.com/Kojoley/atari-py.git

for this to work need to install git and Microsoft Visual C++ Build Tools
https://git-scm.com/
http://landinghub.visualstudio.com/visual-cpp-build-tools







