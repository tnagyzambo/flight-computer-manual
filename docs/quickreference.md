# General Setup
On linux x86 install the latest version of Docker Desktop! Do not just use docker as you will not be able to cross compile properly. 

# Build rOS on x86 for the Pi
1. download the rOS repo
2. Start docker desktop and configure CPU and ram usage
3. under task explorer in vscode
  - run docker build
  - run packer build

>if error pops up related to the git reset --hard <BIG HAS HERE> that means the git depth is too low as the version is too old. This fix is to go 
[this website](https://cdn.kernel.org/pub/linux/kernel/projects/rt/5.15/)
and take note of the 5.15.XX-rtYY. Check the [raspberrypi linux repo](https://github.com/raspberrypi/linux/tree/rpi-5.15.y). Click on the commits 
and scroll down until you fine the commit titled Linx 5.15.(XX+1) (so if XX from the RT patch is 71 you are looking for the first 72 commit.)
The commit before this is the last commit that will work with the rt Patch. Copy the full SHA and replace it in the dockerbuild file on line 23.
You will aslo need to update the RT_VERSION to the latest 5.15.XX-rtYY you are using on line 39. 

>You can check that the version in that commit by going to commit details. Browse files and then scroll down to the Makefile. It should have the have a 
a lower version. So 71 in this case.

4. flash the new image (and maybe save it for later) to the pi SD card using balena etcher or the pi etcher. You can find the image in the rOS repo.
5. Perform first boot for 5-10 minutes

You now have a pi with a fresh install of rOS!
  
# Compiling rCTRL
Again you will need docker desktop up and running for this.  Check vscode task explorer for these commands.
1. Open rCTRL in dev container (this will take a while if its the first time in a while)
2. Build ui in dev container as its web assembly
3. ROS needs to be built in the xcompile if you a non-arm cpu
  
>if permission issues: check if files are owned by you (ls -l) if they are owned by root. Exit the dev container. Comment out remote user: ros in dev-container.json and use chown -R ros /workspaces
then exit back out. uncomment the user line and re open dev container. Check to ensure you now own files as ros user

# Uploading new rCTRL settings to Pi
1. follow networking commands in the networking doc to get your network adapter information
2. enable network sharing 
3. use `ifconfig` to find the flight computer ip
4. connect to it by using fancy command and change user and ip in said command
5. sync things. password pop up is in terminal. This is done in vscode task explorer again
6. make sure dev container doesn't have open ports
7. in your browser open:
  1. `localhost:8080`
  2. `localhost:8086`
8. influxdb: user & password
9. use the ssh terminal to do stuff
10. `source install/setup.bash && ros2 launch launch/launch.xml`

## influxdb  
1. boards: use this to setup views
2. to get data out:
3. Data: find the data you want
4. set window period to around 1ms and past hour and then download as CSV

## for auto sequennce config:
- all fields need to be present
- set -1 if you do not want the event to occur.
- doesn't matter if open is before or after
- don't touch default level. not a great plan. sets bootup state
- **currently open = powered**
- **closed = unpowered**
- open in a new terminal without port commands
- gpioinfo to check stuff
