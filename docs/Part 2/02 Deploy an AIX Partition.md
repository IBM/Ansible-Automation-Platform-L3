![type:video](./_videos/IBM Power Systems Virtual Server Level 3 - Introduction.mp4)
!!! tip "Ways to Watch"
    In addition to the embedded video, IBMers and Business Partners can also <a href="https://ibm.seismic.com/Link/Content/DCGdHJ7DMdqHD8cV7Wp8f4Rg9Bgd" target="_blank">download the recording from Seismic</a>.

The deployment of an AIX operating system partition on to the PowerVC infrastructure will be automated entirely via the Ansible control (master) node. Ansible will make use of the inventory we've defined, a Playbook that we will modify to paint the broad strokes for what automation is to be performed, and built-in Modules that are part of Ansible's engine to carry out those instructions. Ansible OpenStack Modules— downloaded in the previous step —will allow Ansible to create an LPAR (logical partition) on the PowerVC infrastructure where the AIX operating system can reside.

Begin modification of the OpenStack configuration file (located within your home directory on the Ansible control node) using the following instruction:
```
vi clouds.yaml
```

If you cloned the Git repository earlier, a template has already been crafted for you; otherwise, you will need to recreate it using the sample code below.

Regardless of the route you take, you will need to modify the template to the specifications of your ITZ credentials and environment. The value **idXXXXXXXX** must be replaced with the *username/ID* that was recorded from the Project Kit page.

The same holds true for other variables or fields that have been bolded below in the sample script: if it is highlighted in red text within the screenshot, you must modify the clouds.yaml script for those fields to your own specifications and then save the changes before moving on.
!!! note
    Remember: **ESC** and then **:x** followed by the **Return** key to save your work and exit the VI editor.

The *clouds.yaml* template should look like the following screenshot. Modify as necessary to match the specifications of your unique ITZ environment. Update the userID, password, project name (*ansiblewas*), and the host address (this address should match the PowerVC GUI address from the Project Kit).

![](_attachments/part2_figure1.png)
