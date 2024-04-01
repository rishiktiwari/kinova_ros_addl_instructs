This is for Kinova Gen3 Lite running Firmware 2.2.0

### Downloading the API Files

[Official Video Guide](https://www.youtube.com/watch?v=JparQ69LMzY)

[Visit](https://artifactory.kinovaapps.com/ui/repos/tree/General/generic-public/kortex/API/2.2.0)


Download the following:
	kortex_api-2.2.0.post31-py3-none-any.whl
	linux_x86-64_x86_gcc.zip

```sh
cd ~
git clone https://github.com/Kinovarobotics/kortex.git
```
#### Install C++ API
Extract the downloaded .zip file and move the content to ~/kortex/api_cpp/examples/kortex_api/

#### Install python API
```sh
cd ~/Downloads
python3 -m pip install kortex_api-2.2.0.post31-py3-none-any.whl
```
