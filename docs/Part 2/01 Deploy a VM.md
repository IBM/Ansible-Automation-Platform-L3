![type:video](./_videos/IBM Power Systems Virtual Server Level 3 - Introduction.mp4)
!!! tip "Ways to Watch"
    In addition to the embedded video, IBMers and Business Partners can also <a href="https://ibm.seismic.com/Link/Content/DCGdHJ7DMdqHD8cV7Wp8f4Rg9Bgd" target="_blank">download the recording from Seismic</a>.

Ansible, and in particular **YAML** (Yet Another Markup Language), is very particular about indentation and formatting — something as trivial as an extra whitespace or an incorrectly-indented line of code can cause the interpreter to parse the instructions differently, resulting in outcomes you may not desire.

To streamline the lab as much as possible, a Git repository was created ahead of time with large portions of the Ansible scripts and constructs prepared ahead of time. You will still need to edit elements in each of these files, and the instructions to follow will guide you step by step (as though you were creating these documents from scratch yourself), but it's worth noting that by cloning the Git repository you will be saving yourself a lot of extra typing (and potential debugging). Everything is documented for you— so you can go about creating all of these documents and scripts from scratch, if you wish.

!!! warning
    If you are a **SELLER**, it is strongly recommend that you clone the Git repository, as it will save you time and keystrokes.
    If you are a **TECHNICAL SELLER**, it is recommended that you take the time to craft these files from scratch yourself and that you *do not* clone the Git repository (skip the next two steps).

**SELLERS**: Submit the following instruction to clone the prepared Git repository into your Ansible controller node:
```
git clone --branch hybridcloudevent https://github.com/stephannavarro/ansiblewas.git
```

After the clone action has been completed, navigate to the newly-created ansiblewas directory.
```
cd ansiblewas
```

!!! note
    **TECHNICAL SELLERS**: You will need to manually create the *ansiblewas* directory yourself before continuing. This step was automatically performed for Sellers when they cloned the Git repository. You can do so by issuing the following command:
    ```
    mkdir ansiblewas
    ```

The next order of business is configuring the Ansible controller node (to which you are currently connected) with details about our PowerVC environment and how our project directories are organized. This can be done by modifying the **ansible.cfg** configuration file:
```
vi ansible.cfg
```

!!! note
    Throughout this lab, you will make extensive use of the VI text editor. If you're not familiar with this editor, you can quickly get up to speed using the following cheat sheet: <a href="https://www.cse.scu.edu/~yfang/coen11/vi-CheatSheet.pdf" target="_blank">https://www.cse.scu.edu/~yfang/coen11/vi-CheatSheet.pdf</a> for reference.

As a quick primer on how to use the **VI** editor:

- Navigate using the up/down and left/right arrows keys. Hover the blinking indicator over the point in the text you wish to modify.

- To start adding or deleting text, first press the I (as in "indigo") key. Then either start adding text as normal (you can also paste lines of code that you have copied to your clipboard using CTRL+V) or deleting text using backspace.

- Save your changes and exit a file by first pressing the ESC key, then typing :x (the full colon must come first) and hitting return. If you want to exit a file without saving changes, press ESC and type :quit! (with the exclamation point included at the end).

- Other shortcuts and commands are detailed in the cheat sheet linked above.

Within the configuration file, you will see three values of note: *inventory*, *remote_tmp*, and *host_key_checking*. Their respective purposes are as follows:

- **inventory** will instruct Ansible on which directory the host file should be defined (we will be modifying this file shortly).

- **remote_tmp** instructs which directory to use on the remote system (needed by the AIX operating system).

- **host_key_checking** tells SSH to accept remote keys (which will streamline the lab for us later).

The *ansible.cfg* definition is provided below for those who are crafting these files from scratch (instead of using the cloned Git repository). You **do not** need to make any modification to the *ansible.cfg* file. Use the *ESC + :quit!* command in the VI editor to exit the file without making changes.

```
[defaults]
inventory       = ./hosts
remote_tmp = /tmp
host_key_checking = False
```

We are now ready to install the Ansible OpenStack Python modules on to the Ansible controller node. The **openstacksdk** library is required in order for Ansible to be able to interact with the PowerVC infrastructure provisioned via the ITZ. The following command will take care of installing all of the necessary drivers and dependencies:
```
pip3 install --user "openstacksdk==0.51.0" "python-openstackclient==5.4.0" dnspython dig
```

With the dependencies taken care of, we're now ready to begin deployment of an AIX operating system partition atop of the PowerVC infrastructure provisioned earlier.
