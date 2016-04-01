Import('env')

import os

localEnv = env.Clone()

#============================ defines =========================================

#============================ helpers =========================================

def syscall(cmd):
    print '>>> {0}'.format(cmd)
    os.system(cmd)

#============================ SCons actions ===================================

def ActionBuild(env,target,source):
    
    #===== NOOBS get
    
    syscall("sudo rm -Rf build/")
    syscall("cp {0} .".format(localEnv['OW_PATH_NOOBS_IN']))
    syscall("mkdir build")
    syscall("unzip NOOBS_v1_9_0.zip -d build/")
    syscall("rm -Rf NOOBS_v1_9_0.zip")
    
    #===== NOOBS clean-up
    
    syscall("rm -Rf build/os/Arch")
    syscall("rm -Rf build/os/OpenELEC")
    syscall("rm -Rf build/os/Pidora")
    syscall("rm -Rf build/os/RaspBMC")
    syscall("rm -Rf build/os/RISC_OS")
    
    #===== NOOBS marketing
    
    # default logo
    syscall("cp bits_n_pieces/openwsn_logo.png build/defaults/slides/A.png")
    
    # rename distribution
    syscall("mv build/os/Raspbian build/os/OpenRoverPi")
    
    # installation options
    syscall("cp bits_n_pieces/os.json       build/os/OpenRoverPi/os.json"),
    syscall("cp bits_n_pieces/flavours.json build/os/OpenRoverPi/flavours.json")
    
    # installation logo
    syscall("rm build/os/OpenRoverPi/Raspbian.png")
    syscall("cp bits_n_pieces/OpenRoverPi.png    build/os/OpenRoverPi/OpenRoverPi.png")
    
    # installation intro slides (shown during installation)
    syscall("rm -Rf build/os/OpenRoverPi/slides_vga")
    syscall("cp -r bits_n_pieces/slides_vga build/os/OpenRoverPi/slides_vga")
    
    #===== OpenRoverPi customization
    
    # extract root
    syscall("sudo mkdir build/os/OpenRoverPi/root/")
    syscall("sudo tar -xJf build/os/OpenRoverPi/root.tar.xz -C build/os/OpenRoverPi/root/")
    syscall("sudo rm -Rf build/os/OpenRoverPi/root.tar.xz")
    
    # change desktop background image
    syscall("sudo rm build/os/OpenRoverPi/root/etc/alternatives/desktop-background")
    syscall("sudo cp bits_n_pieces/desktop-background build/os/OpenRoverPi/root/etc/alternatives/desktop-background")
    
    # install python module dependencies (bottle, PyDispatcher, pyzmq)
    syscall("wget https://pypi.python.org/packages/source/b/bottle/bottle-0.12.7.tar.gz")
    syscall("tar -zxvf bottle-0.12.7.tar.gz")
    syscall("sudo cp bottle-0.12.7/bottle.py build/os/OpenRoverPi/root/usr/local/lib/python2.7/dist-packages/")
    syscall("sudo mv bottle-0.12.7/bottle.py build/os/OpenRoverPi/root/usr/local/bin/")
    syscall("sudo rm bottle-0.12.7.tar.gz")
    syscall("sudo rm -Rf bottle-0.12.7/")
    syscall("wget https://pypi.python.org/packages/source/P/PyDispatcher/PyDispatcher-2.0.3.tar.gz")
    syscall("tar -zxvf PyDispatcher-2.0.3.tar.gz")
    syscall("sudo mv PyDispatcher-2.0.3/pydispatch build/os/OpenRoverPi/root/usr/local/lib/python2.7/dist-packages/")
    syscall("sudo rm PyDispatcher-2.0.3.tar.gz")
    syscall("sudo rm -Rf PyDispatcher-2.0.3/")
    syscall("wget https://pypi.python.org/packages/source/p/pyzmq/pyzmq-15.1.0.tar.gz")
    syscall("tar -zxvf pyzmq-15.1.0.tar.gz")
    syscall("sudo mv pyzmq-15.1.0/zmq build/os/OpenRoverPi/root/usr/local/lib/python2.7/dist-packages/")
    syscall("sudo rm pyzmq-15.1.0.tar.gz")
    syscall("sudo rm -Rf pyzmq-15.1.0/")

    # install OpenRover
    syscall("wget https://codeload.github.com/zach-b/openrover/zip/master")
    syscall("unzip master")
    syscall("sudo rm master")
    syscall("sudo mv openrover-master openrover")
    syscall("sudo mv openrover build/os/OpenRoverPi/root/home/pi/")
    syscall("sudo cp bits_n_pieces/openrover build/os/OpenRoverPi/root/etc/init.d")
    syscall("sudo chmod +x build/os/OpenRoverPi/root/etc/init.d/openrover")

    # install OpenWSN-SW
    syscall("wget https://codeload.github.com/openwsn-berkeley/openwsn-sw/zip/develop")
    syscall("unzip develop")
    syscall("sudo rm develop")
    syscall("sudo mv openwsn-sw-develop openwsn-sw")
    syscall("sudo mv openwsn-sw build/os/OpenRoverPi/root/home/pi/")

    # update modules to run
    syscall("sudo cp bits_n_pieces/modules build/os/OpenRoverPi/root/etc/")
    
    # customize boot message, start OpenVisualizer on boot
    # syscall("sudo cp bits_n_pieces/rc.local build/os/OpenRoverPi/root/etc")
    
    # compress root
    syscall("cd build/os/OpenRoverPi/root/ ; sudo tar -cJf ../root.tar.xz ./ ; cd ../../../../")
    syscall("sudo rm -Rf build/os/OpenRoverPi/root/")
    
    #===== OpenRoverPi wrap-up and publish
    
    # create final zip
    syscall("cd build ; zip -r ../OpenRoverPi.zip * ; cd ..")
    syscall("rm -Rf build")
    
    # copy to final location
    syscall("mv OpenRoverPi.zip {0}".format(localEnv['OW_PATH_OPENROVERPI_OUT']))

#============================ SCons targets ===================================

build = localEnv.Command(
    'dummy.out',[],
    [
        ActionBuild,
    ]
)

localEnv.Alias('build', build)
