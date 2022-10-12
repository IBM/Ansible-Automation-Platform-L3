![type:video](./_videos/Part 2a - Deploy a Virtual Machine - Red Hat Ansible Automation Platform Level 3 for Sales and Technical Sales.mp4)
!!! tip "WAYS TO WATCH"
    In addition to the embedded video, IBMers and Business Partners can also <a href="https://ibm.seismic.com/Link/Content/DC3cV2Vm8jFgV8227jV8M3BcGgCV" target="_blank">download the recording from Seismic</a>.
!!! note "VI CHEAT SHEET"
    Throughout this lab, you will make extensive use of the VI text editor. If you're not familiar with this editor, you can quickly get up to speed using the following cheat sheet: <a href="https://www.cse.scu.edu/~yfang/coen11/vi-CheatSheet.pdf" target="_blank">https://www.cse.scu.edu/~yfang/coen11/vi-CheatSheet.pdf</a> for reference.

Ansible, and in particular **YAML** (Yet Another Markup Language), is very particular about indentation and formatting — something as trivial as an extra whitespace or an incorrectly-indented line of code can cause the interpreter to parse the instructions differently, resulting in outcomes you may not have intended.

To streamline the lab as much as possible, a Git repository was created ahead of time with large portions of the Ansible scripts and constructs prepared ahead of time. You will still need to edit elements in each of these files, and the instructions to follow will guide you step by step (as though you were creating these documents from scratch yourself), but it's worth noting that by cloning the Git repository you will be saving yourself a lot of extra typing (and potential debugging). Everything is documented for you— so you can go about creating all of these documents and scripts from scratch, if you wish. Note that the cloned repository will be replicated to your ITZ virtual machine, not your local machine.

#
# If you are a **SELLER**, it is strongly recommended that you clone the Git repository, as it will save you time and keystrokes.
# If you are a **TECHNICAL SELLER**, it is recommended that you take the time to craft these files from scratch yourself and that you *do not* clone the Git repository.

!!! tip "SELLERS"
    Submit the following instruction to clone the prepared Git repository into your Ansible controller node:
    ```
    git clone --branch hybridcloudevent https://github.com/stephannavarro/ansiblewas.git
    ```

!!! warning "TECHNICAL SELLERS"
    You will need to manually create the *ansiblewas* directory yourself before continuing. This step was automatically performed for Sellers when they cloned the Git repository. You can do so by issuing the following instruction:
    ```
    mkdir ansiblewas
    ```

!!! note "ALL USERS"
    Navigate to the newly-created *ansiblewas* directory.
    ```
    cd ansiblewas
    ```

As a quick primer on how to use the **VI** editor:

- Navigate using the up/down and left/right arrows keys. Hover the blinking indicator over the point in the text you wish to modify.

- To start adding or deleting text, first press the ```I``` (as in "indigo") key and then begin adding text as normal. You can also paste lines of code that you have copied to your clipboard using CTRL+V (useful for the code blocks peppered throughout this documentation) or delete text using ```Backspace```.

- Save your changes and exit a file by first pressing the ```ESC``` key, then typing ```:x``` (the full colon must come first) and hitting ```Return```. If you want to exit a file without saving changes, press ```ESC``` and type ```:quit!``` (with the exclamation point included at the end) followed by ```Return```.

- Other shortcuts and commands are detailed in the cheat sheet linked above.

Within the configuration file, you will see three values of note: *inventory*, *remote_tmp*, and *host_key_checking*. Their respective purposes are as follows:

- **inventory** will instruct Ansible on which directory the host file should be defined (we will be modifying this file shortly).

- **remote_tmp** instructs which directory to use on the remote system (needed by the AIX operating system).

- **host_key_checking** tells SSH to accept remote keys (which will streamline the lab for us later).

The next order of business is configuring the Ansible controller node (to which you are currently connected) with details about our PowerVC environment and how our project directories are organized. This can be done by modifying the **ansible.cfg** configuration file:
```
vi ansible.cfg
```

The *ansible.cfg* definition is provided below. for Technical Sellers who are crafting these files from scratch (instead of using the cloned Git repository). Sellers **do not** need to make any modification to the *ansible.cfg* file. Enter ```ESC``` followed by ```:quit!``` and ```Return``` in the VI editor to exit the file without making any changes to the code.

```
[defaults]
inventory       = ./hosts
remote_tmp = /tmp
host_key_checking = False
```
!!! tip "SELLERS"
    As you are working from a cloned Git repository, the *ansible.cfg* file is already defined within your directory. You can inspect it using the ```vi ansible.cfg``` command mentioned above, but you do not need to modify or adjust the values inside the configuration file. Exit the VI editor without saving changes by pressing ```ESC``` followed by ```:quit!``` and ```Return```.

!!! warning "TECHNICAL SELLERS"
    As you are crafting these files from scratch, you will need to copy the code *ansible.cfg* code block referenced above and save it by pressing ```ESC``` followed by ```:x``` and ```Return```.


We are now ready to install the Ansible OpenStack Python modules on to the Ansible controller node. The **openstacksdk** library is required in order for Ansible to be able to interact with the PowerVC infrastructure provisioned via the ITZ. The following command will take care of installing all of the necessary drivers and dependencies:
```
pip3 install --user "openstacksdk==0.51.0" "python-openstackclient==5.4.0" dnspython dig
```

With the dependencies taken care of, we're now ready to begin deployment of an AIX operating system partition atop of the PowerVC infrastructure provisioned earlier.
