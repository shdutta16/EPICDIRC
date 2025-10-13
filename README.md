# EPIC DIRC

## Setting up the framework
```curl --location https://get.epic-eic.org | bash```

The above command gave the following error:
```ERROR: no singularity found, please make sure you have singularity in your $PATH```
Full output after the command:
```
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
  0     0    0     0    0     0      0      0 --:--:--  0:00:02 --:--:--     0
100 14134  100 14134    0     0   6123      0  0:00:02  0:00:02 --:--:--  6123
~/G4WORK/EIC/EICDIRC/epicdirc ~/G4WORK/EIC/EICDIRC/epicdirc
Setting up development environment for eicweb/eic_xl:nightly
 - Detected OS: Linux
 - Detected CPU: x86_64
ERROR: no singularity found, please make sure you have singularity in your $PATH
```

Installed singularity using ```sudo apt-get install singularity```
After installing singularity the above previous error was gone. 

Now facing the following error:
```
Destination: /tmp/ci-2EtXRfe0up/lib/eic_xl-nightly.sif
WARNING: failed to retrieve container artifact
Attempting alternative download from docker registry
Executing: singularity pull --force /tmp/ci-2EtXRfe0up/lib/eic_xl-nightly.sif docker://eicweb/eic_xl:nightly
Singularity 1.0b1 (commit: b0d3406e311f4ac3aa6f4d0187f32926b3808fef)
Running under Python 3.8.10 (default, Mar 18 2025, 20:04:55) [GCC 9.4.0]
pygame 1.9.6
Hello from the pygame community. https://www.pygame.org/contribute.html
The error-log configured as /home/shubhamdutta/.local/share/singularity/log/error.log (lazily created when something is logged)
Usage: singularity [options]

singularity: error: no such option: --force
Traceback (most recent call last):
  File "/usr/lib/python3.8/urllib/request.py", line 1354, in do_open
    h.request(req.get_method(), req.selector, req.data, headers,
  File "/usr/lib/python3.8/http/client.py", line 1256, in request
    self._send_request(method, url, body, headers, encode_chunked)
  File "/usr/lib/python3.8/http/client.py", line 1302, in _send_request
    self.endheaders(body, encode_chunked=encode_chunked)
  File "/usr/lib/python3.8/http/client.py", line 1251, in endheaders
    self._send_output(message_body, encode_chunked=encode_chunked)
  File "/usr/lib/python3.8/http/client.py", line 1011, in _send_output
    self.send(msg)
  File "/usr/lib/python3.8/http/client.py", line 951, in send
    self.connect()
  File "/usr/lib/python3.8/http/client.py", line 1418, in connect
    super().connect()
  File "/usr/lib/python3.8/http/client.py", line 922, in connect
    self.sock = self._create_connection(
  File "/usr/lib/python3.8/socket.py", line 787, in create_connection
    for res in getaddrinfo(host, port, 0, SOCK_STREAM):
  File "/usr/lib/python3.8/socket.py", line 918, in getaddrinfo
    for res in _socket.getaddrinfo(host, port, family, type, proto, flags):
socket.gaierror: [Errno -5] No address associated with hostname

During handling of the above exception, another exception occurred:

Traceback (most recent call last):
  File "./install.py", line 307, in <module>
    urllib.request.urlretrieve(url, container)
  File "/usr/lib/python3.8/urllib/request.py", line 247, in urlretrieve
    with contextlib.closing(urlopen(url, data)) as fp:
  File "/usr/lib/python3.8/urllib/request.py", line 222, in urlopen
    return opener.open(url, data, timeout)
  File "/usr/lib/python3.8/urllib/request.py", line 525, in open
    response = self._open(req, data)
  File "/usr/lib/python3.8/urllib/request.py", line 542, in _open
    result = self._call_chain(self.handle_open, protocol, protocol +
  File "/usr/lib/python3.8/urllib/request.py", line 502, in _call_chain
    result = func(*args)
  File "/usr/lib/python3.8/urllib/request.py", line 1397, in https_open
    return self.do_open(http.client.HTTPSConnection, req,
  File "/usr/lib/python3.8/urllib/request.py", line 1357, in do_open
    raise URLError(err)
urllib.error.URLError: <urlopen error [Errno -5] No address associated with hostname>

During handling of the above exception, another exception occurred:

Traceback (most recent call last):
  File "./install.py", line 316, in <module>
    raise ContainerDownloadError()
__main__.ContainerDownloadError
mv: cannot stat 'lib/eic_xl-nightly.sif': No such file or directory
chmod: cannot access '/home/shubhamdutta/G4WORK/EIC/EICDIRC/epicdirc/local/lib/eic_xl-nightly.sif': No such file or directory
~/G4WORK/EIC/EICDIRC/epicdirc ~/G4WORK/EIC/EICDIRC/epicdirc
/home/shubhamdutta/G4WORK/EIC/EICDIRC/epicdirc/local/lib/eic_xl-nightly.sif
ls: cannot access '/home/shubhamdutta/G4WORK/EIC/EICDIRC/epicdirc/local/lib/eic_xl-nightly.sif': No such file or directory
ERROR: no singularity image found
```
**TL;DR -> The option --force is unavailable in singularity that I installed earlier.** From chatgpt got the following recommendation: 

1. Why the error mentions Singularity
Many HEP tools (e.g. CMS Combine, CRAB, or CMSSW utilities) still reference the command singularity for backward compatibility.
However, the original Singularity project has since split into two separate branches:
| Project           | Maintainer       | Current Binary | Status                              |
| ----------------- | ---------------- | -------------- | ----------------------------------- |
| **Apptainer**     | Linux Foundation | `apptainer`    | ✅ Official open-source continuation |
| **SingularityCE** | Sylabs, Inc.     | `singularity`  | ⚙️ Commercial/community fork        |
So, when a script reports an error mentioning Singularity, it doesn’t necessarily mean that you need the older “SingularityCE” package — it simply expects a compatible container runtime (which Apptainer provides).

2. Why Apptainer is recommended
* Apptainer is the actively maintained and fully open-source successor to Singularity, now under the Linux Foundation.
* SingularityCE is a commercial/community fork maintained by Sylabs.
* Both provide nearly identical command-line interfaces, so you can safely alias:
  ```ln -s $(which apptainer) /usr/local/bin/singularity```
  to ensure full compatibility with tools that still expect the old singularity command name.
**TL;DR -> Installing Apptainer ensures compatibility with modern HEP software while avoiding legacy issues.**

  Installing **apptainer** after removing singularity:
  * Remove singularity: ```sudo apt-get remove singularity``` 
  * Add the Apptainer repository: ```sudo add-apt-repository ppa:apptainer/ppa``` (this was needed since apptainer wasn't available in the default Ubuntu repositories) 
  * Install Apptainer: ```sudo apt-get install apptainer```
