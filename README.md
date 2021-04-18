***Ultimate android Image***

It's a docker image, provides an easy way to have an android emulator working inside a docker container by running few simple commands, the image as well comes with the latest Android SDK and the latest Appium version to ease the CI/CD, Aslo selenium service is added so you can run you automation script against chrome browser.

Emulator image is x86 CPU to enhance its speed and performance, also google play services have been added.

**Setup** 

1. pull the docker image: *docker pull amrka/ultimate-android:latest*

2. Start your container : *docker run -it -d -p 5900:5900 --name androidImage --privileged amrka/ultimate-android*

3. Launch the appium session: *docker exec --privileged  -it androidImage bash -c "appium -p 5900"*

4. Start the emulator in headless mode : *docker exec --privileged  -it androidImage bash -c "./root/start_emu_headless.sh"*


**Running the emulator in VNC**

1. You will have to start the docker container by using the following command : 

*docker run -it -d -p 5900:5900 --name androidImage -e VNC_SERVER_PASSWORD=password  --privileged amrka/ultimate-android*

2. Instantiate the vnc service by running : 

*docker exec --privileged  -it androidImage bash -c "./root/bootstrap.sh"*

3. Connect to the VNC server via remmina or any VNC viewer

4. Open dash terminal and right the following command:  ./root/start_emu.sh

Note: start_emu.sh will launch the emulator in headed mode, so this shell script should be used when integration with the pipeline (e.g. Jenkins)
 use start_emu_headless.sh instead.

**Cloning a git Repository**

Basically, we are running the same command mentioned above except that we are passing different Env variables, so to clone a repo you will need to pass the following env
while starting the container:

- branch:      Repository branch name
- username: GitHub username (not your email)
- gitPass:      Github password
- gitUrl:        Repository url (without https://   e.g. github.com/amrka/androidImage.git

docker run --privileged -it -d -p 5900:5900 --name androidImage -e branch=win -e username=amrka -e gitPass=test -e gitUrl=github.com/amrka/Docker-Images.git amrka/ultimate-android

After running the command above you will be able now to run the following command to have  your
Repository clones in / directory 

docker exec --privileged -it androidImage bash -c "./root/clone_repo.sh"

Note: You can combine all VNC env var with the env above.

**Using Docker-compose**

Using the dokcer-compose file will make initiating the service even more easier, docker-compose yml file provided has two main services android emulator with appium and selenium service, you can tweak them or adjust the file based on you need


**Kill the container**
1. Run the following command to kill and remove the container : *docker rm -f  androidImage
