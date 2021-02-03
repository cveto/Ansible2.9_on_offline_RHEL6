# Ansible 2.9.5 on an offline_RHEL6 machine
How to get newer version of ansible on RHEL6 by installing python2.7 and virutalenv


**GOAL**:  		To get newer versions of ansible work on RHEL6

**PROBLEM**:		
 - Newer versions of Ansible require python 2.7
 - Newer versions of Ansible are not supported on RHEL6 any longer.
 - Existing software on RHEL6 monitoring machines relies on python 2.6
 - Existing machine is not connected to the internet.
			
**SOLUTION**:	Install Python 2.7 next to Python 2.6 in a way not to disturb any existing services

## SET UP an offline YUM REPOSITORY
```
# cp -r 1_offlinerepo /root/offlinerepo
# cp offlinerepo/offline.repo /etc/yum.repos.d/
# yum repolist
```


## INSTALL DEPENDENCIES FOR COMPLIING CODE
**ONLINE *
```
# yum groupinstall "Developement tools"
```

 **OFFLINE*
```							
# yum install --disablerepo='*' --enablerepo='offlinerepo' \
autoconf automake bison byacc cscope \
ctags diffstat doxygen flex gcc gcc-c++ \
gcc-gfortran git indent intltool libtool \
patchutils rcs redhat-rpm-config rpm-build \
subversion swig systemtap binutils elfutils make patch
```
							
## INSTALL SOME OTHER DEPENDENIES to avoid recompiling Python2.7 in the future.
								
```
# yum install --disablerepo='*' --enablerepo='offlinerepo' \
zlib-devel bzip2-devel openssl-devel ncurses-devel \
db4-devel sqlite-devel readline-devel tk-devel gdbm-devel
```
	
## INSTALL PYTHON
```
# cd 2_python-2.7.5
# tar -xf Python-2.7.5.tar.xz
# cd Python-2.7.5
# ./configure
# make 
# make altinstall

# python2.7 --version
# python --version  			(should still show 2.6)
```
			

## Install the single dependecy for PIP
```
# cd \3_setup-tools\setuptools-44.1.1
# python2.7 setup.py install
```

## INSTALL PIP with python 2.7

**ONLINE**
```
 # cd 4_pip
 # python2.7 get-pip.py install --no-setuptools --no-wheel
```
  
**OFFLINE**
```
# cd 4_pip
# python2.7 pip-20.3.4-py2.py3-none-any.whl/pip install --no-index  pip-20.3.4-py2.py3-none-any.whl
```

			
## Install VirtualENV as to be able to install Ansible without breaking anything
***ONLINE***
```
# pip install virtualenv
```
  
**OFFLINE**
```
# cd virtualenv
# pip install --no-index --find-links=. -r requirements.txt
```

## Configure virtual environemnt
```
# cd ~
# mkdir ansible_virutal
# virtualenv ansible_virtual
# source ansible_virtual/bin/activate
```

### Install ansible whichever version in the virtual environment
```
# cd 5_pip_packages/5.2_ansible/ansible-2.9.2
# pip install --no-index --find-links=. -r requirements.txt --no-build-isolation
```

### Exiz the virtual environment
```
# deactivate
```
