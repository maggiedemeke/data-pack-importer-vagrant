### Vagrant

Installer for [data-pack-importer](https://github.com/jason-p-pickering/data-pack-importer)

Vagrant isolates all dependencies and requirements into a Virtual Machine (Ubuntu 16.04). The "host" is your local computer while the "guest" is a isolated VM. 

This is meant to run on almost any system. However, you will need a substantial amount of RAM (4 GB) and disk space (5 GB) - if you're on a slow computer (i.e. only 4GB RAM), see [chapter "Other"](#other) for details.

1. Install [VirtualBox](https://www.virtualbox.org/wiki/Downloads).
2. Install [Vagrant](https://www.vagrantup.com/downloads).
3. If you're on Windows 7, [update Powershell](https://docs.microsoft.com/en-us/powershell/scripting/setup/installing-windows-powershell?view=powershell-6#upgrading-existing-windows-powershell) to v4 ([ref](https://github.com/hashicorp/vagrant/issues/8777))
4. Clone this repository
5. Open a terminal and change into repository i.e. with `cd /path/to/repo`
6. In the terminal: `vagrant box add ubuntu/xenial64`
7. In the terminal: `vagrant up` (this takes a while if doing it for the first time) until finished. Eventually you should see e.g. `==> default: Notice: Finished catalog run in 123.45 seconds`
8. Open web browser: http://localhost:8787, username: `vagrant` password: `vagrant`
9. The location for shared files between host and guest is within the same repository. 

## Run

1. Download support files from [Sharepoint](https://www.pepfar.net/Project-Pages/collab-38/Shared%20Documents/Forms/AllItems.aspx?RootFolder=%2FProject-Pages%2Fcollab-38%2FShared%20Documents%2FCOP18%20Target%20Setting%20Process%20Improvement%2FImport%20Team&FolderCTID=0x012000C4AC9B35DC4AB84FAEEF47AE703A28CE00C799CA85D140EF45960B9C47CE99E19F&View=%7BA8BAC8D0-846B-4EFE-8763-758855081F5D%7D&InitialTabId=Ribbon%2EDocument&VisibilityContext=WSSTabPersistence#InplviewHasha8bac8d0-846b-4efe-8763-758855081f5d=RootFolder%3D%252FProject%252DPages%252Fcollab%252D38%252FShared%2520Documents%252FCOP18%2520Target%2520Setting%2520Process%2520Improvement%252FImport%2520Team) and place it into `/path/to/repo/support_files/`

2. Download DisaggTool spreadsheets from Support Ticket and put it into `/path/to/repo/disagg_tools/`

3. Adjust `distribution_year` to either `2017` or `2018`. This should be indicated in the Support Ticket.

4. Paste it into the RStudio console in http://localhost:8787. If successful, it outputs a Excel file in the `disagg_tools` folder.


```R
# ADJUST THIS
disagg_tool_file="ZambiaCOP18DisaggTool_HTSv2018.02.11.xlsx"
distribution_year=2017

# DO NOT CHANGE
support_files="/vagrant/support_files/"
disagg_tool=paste0("/vagrant/disagg_tools/", disagg_tool_file)
library(devtools)
library(datapackimporter)
wb<-disagg_tool
psnu_data<-ImportSheets(wb,
               distribution_method = distribution_year,
               support_files_path = support_files)
```

### Other

- Check status: `vagrant status`
- To stop vagrant: `vagrant halt` in the host machine
- To update vagrant with code changes from the `data-pack-importer` repository, call either: `install_github(repo="jason-p-pickering/data-pack-importer", ref="prod")` in the R console, or run `vagrant up --provision` from the host machine.
- If you experience that Vagrant takes a lot of CPU, you can change it in the `Vagrantfile` before calling `vagrant up` and set it to `v.cpus = 1` and maybe also `v.memory = 2048`
- To SSH into the guest machine, use `vagrant ssh`
- To see history of RStudio commands, ssh into the machine and open the file e.g. with `less ~/.rstudio/history_database`
