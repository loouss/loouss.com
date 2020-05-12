---
title: vagrant
date: 2020-05-12 10:00:06
tags:
---


## 安装

- 虚拟化平台

    在安装 vagrant 之前需要先安装虚拟化平台，vmware（收费） 或者 virtualBox（免费）。

    - 安装 vmware [官网](https://www.vmware.com/) 下载 .exe 安装包安装vmware。
    - 安装 virtualBox [官网](https://www.virtualbox.org/) 下载 .exe 安装包安装virtualBox。

    > vmware 与 virtualBox 可以任意选择一个

- Vagrant
  
    下载vagrant [下载](https://www.vagrantup.com/downloads.html) 下载对应的系统及系统位数。
    
    > 安装完成后添加box注意选择对应的虚拟化平台

## vagrant 常用命令

##### 使用 `vagrant --help` 查看命令格式及常用命令

```shell

λ vagrant --help                                                                                                 
Usage: vagrant [options] <command> [<args>]                                                                      
                                                                                                                 
    -h, --help                       Print this help.                                                            
                                                                                                                 
Common commands:                                                                                                 
     box             manages boxes: installation, removal, etc.                                                  
     cloud           manages everything related to Vagrant Cloud                                                 
     destroy         stops and deletes all traces of the vagrant machine                                         
     global-status   outputs status Vagrant environments for this user                                           
     halt            stops the vagrant machine                                                                   
     help            shows the help for a subcommand                                                             
     init            initializes a new Vagrant environment by creating a Vagrantfile                             
     login                                                                                                       
     package         packages a running vagrant environment into a box                                           
     plugin          manages plugins: install, uninstall, update, etc.                                           
     port            displays information about guest port mappings                                              
     powershell      connects to machine via powershell remoting                                                 
     provision       provisions the vagrant machine                                                              
     push            deploys code in this environment to a configured destination                                
     rdp             connects to machine via RDP                                                                 
     reload          restarts vagrant machine, loads new Vagrantfile configuration                               
     resume          resume a suspended vagrant machine                                                          
     snapshot        manages snapshots: saving, restoring, etc.                                                  
     ssh             connects to machine via SSH                                                                 
     ssh-config      outputs OpenSSH valid configuration to connect to the machine                               
     status          outputs status of the vagrant machine                                                       
     suspend         suspends the machine                                                                        
     up              starts and provisions the vagrant environment                                               
     upload          upload to machine via communicator                                                          
     validate        validates the Vagrantfile                                                                   
     version         prints current and latest Vagrant version                                                   
     winrm           executes commands on a machine via WinRM                                                    
     winrm-config    outputs WinRM configuration to connect to the machine                                       
                                                                                                                 
For help on any individual command run `vagrant COMMAND -h`                                                      
                                                                                                                 
Additional subcommands are available, but are either more advanced                                               
or not commonly used. To see all subcommands, run the command                                                    
`vagrant list-commands`.                                                                                         
        --[no-]color                 Enable or disable color output                                              
        --machine-readable           Enable machine readable output                                              
    -v, --version                    Display Vagrant version                                                     
        --debug                      Enable debug output                                                         
        --timestamp                  Enable timestamps on log output                                             
        --debug-timestamp            Enable debug output with timestamps                                         
        --no-tty                     Enable non-interactive output                                                                                                             

```

##### 管理box

首先可以执行 `vagrant box --help` 命令查看如何管理box

```shell

λ vagrant box --help
Usage: vagrant box <subcommand> [<args>]

Available subcommands:
     add
     list
     outdated
     prune
     remove
     repackage
     update

For help on any individual subcommand run `vagrant box <subcommand> -h`
        --[no-]color                 Enable or disable color output
        --machine-readable           Enable machine readable output
    -v, --version                    Display Vagrant version
        --debug                      Enable debug output
        --timestamp                  Enable timestamps on log output
        --debug-timestamp            Enable debug output with timestamps
        --no-tty                     Enable non-interactive output

```

- 添加 box

```shell

λ vagrant box add laravel/homestead

```

![box下载地址](https://loouss-1252083494.cos.ap-chongqing.myqcloud.com/blog/vagrant1.png)

> 如果 box下载缓慢，可以用其他方式下载box，然后从本地安装box



