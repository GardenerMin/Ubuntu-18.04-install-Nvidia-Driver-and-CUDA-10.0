# Ubuntu-18.04-install-Nvidia-Driver-and-CUDA-10.0
Installing Nvidia Driver and CUDA 10.0 on Ubuntu 18.04. (Runfile installation)
1. Download CUDA (runfile)

   Option 1. From official website https://developer.nvidia.com/cuda-downloads?target_os=Linux&target_arch=x86_64&target_distro=Ubuntu&target_version=1804&target_type=runfilelocal
	 
   Option 2. Or use the following command: 
	 ```
   wget http://developer.download.nvidia.com/compute/cuda/10.1/Prod/local_installers/cuda_10.1.243_418.87.00_linux.run
   ```
	 
2. Close X Server

   The following command sometimes causes failure:    
	 ```
	 sudo service lightdm stop
	 ```
	 
	 Here I suggest to use the following command to close the X Server
	 ```
	 sudo systemctl set-default multi-user.target
	 sudo reboot
	 ```
	 
3. Make sure there is no Nvidia Driver installed on the machine.

   Or use the following command to uninstall the existing Nvidia Driver
	 ```
	 sudo apt-get purge nvidia*
	 ```
	 
4. Disable nouveau
   ```
   sudo gedit /etc/modprobe.d/blacklist-nouveau.conf
	 ```
	 Add "blacklist nouveau" to the end of the file, then
	 ```
	 sudo update-initramfs -u
	 sudo reboot
	 ```
	 After reboot, check if nouveau is successfully disabled
	 ```
	 lsmod | grep nouveau
	 ```
	 If there is no output in the terminal, nouveau is disabled.
	 
5. Install Nvidia Driver and CUDA toolkit
   Under the runfile download path, run
   ```
	 sudo sh cuda_10.1.243_418.87.00_linux.run
	 ```
	 When the installation is done, shown as
	 ```
   ===========
   = Summary =
   ===========
 
   Driver:   Nvidia
   Toolkit:  Installed in /usr/local/cuda-10.0
   Samples:  Installed in /home/fc, but missing recommended libraries
 
   Please make sure that
   -   PATH includes /usr/local/cuda-10.0/bin
   -   LD_LIBRARY_PATH includes /usr/local/cuda-10.0/lib64, or, add /usr/local/cuda-10.0/lib64 to /etc/ld.so.conf and run ldconfig as root
	 ```

6. Modify ~./bashrc and /etc/profile

   First, Modify bashrc file
   sudo gedit ~/.bashrc
   ```
	 """
   Modify these two lines
   """
   export PATH=/usr/local/cuda-10.0/bin:$PATH
   export LD_LIBRARY_PATH=/usr/local/cuda-10.0/lib64:$LD_LIBRARY_PATH
   """
   wq！
   """
   source ~/.bashrc
	 ```
   Then, modify /etc/profile
	 ```
	 sudo vim /etc/profile
	 """
   modify this line
   """
   export PATH=/usr/local/cuda-10.0/bin:$PATH
	 ```
	 Then, cuda.conf file
	 ```
	 sudo vim /etc/ld.so.conf.d/cuda.conf
   """
   write to file
   """
   /usr/local/cuda/lib64 
   """
   wq!
	 """
	 ```
	 Finally, 
	 ```
	 sudo ldconfig 
	 ```
	 
7. Start X server
   ```
	 sudo systemctl set-default graphical.target
   sudo reboot
	 ```
	 
	 Done!

	 
