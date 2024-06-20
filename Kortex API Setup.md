This is for Kinova Gen3 robotic arm connected to computer running [Ubuntu 18](https://releases.ubuntu.com/18.04/).

Instructions are tested for Kortex API 2.2.0, newer versions may work using the same instructions.

## Downloading the API Files

[Official Video Guide](https://www.youtube.com/watch?v=JparQ69LMzY) | [API Repository](https://artifactory.kinovaapps.com/ui/repos/tree/General/generic-public/kortex/API/2.2.0)


Download the following from API repository:
- kortex_api-2.2.0.post31-py3-none-any.whl
- linux_x86-64_x86_gcc.zip

And clone the Kortex repository:

```sh
cd ~
git clone https://github.com/Kinovarobotics/kortex.git
```
## Install C++ API
Extract the downloaded linux_x86-64_x86_gcc.zip file and move the content to `~/kortex/api_cpp/examples/kortex_api/`

## Install python API
Navigate to directory where *kortex_api-2.2.0.post31-py3-none-any.whl* is downloaded.

Generally should be: `cd ~/Downloads`

```sh
python3 -m pip install kortex_api-2.2.0.post31-py3-none-any.whl
```
