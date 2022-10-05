![type:video](./_videos/IBM Power Systems Virtual Server Level 3 - Introduction.mp4)
!!! tip "Ways to watch"
    In addition to the embedded video, IBMers and Business Partners can also <a href="https://ibm.seismic.com/Link/Content/DCGdHJ7DMdqHD8cV7Wp8f4Rg9Bgd" target="_blank">download the recording from Seismic</a>.

The first task that Ansible's automation will need to tackle is creation of a staging directory, where the **WAS Installation Manager binaries** can be downloaded and unpackaged. Begin by using the VI editor to create (or modify/examine) the *main.yml* Roles via the following command:

```
vi roles/was/tasks/main.yml
```

The */tmp/im* directory will be the path designated for this purpose.
!!! note "6 GB Scaling"
    In an earlier section of this lab, you'll recall that we increased the size of this directory to 6 GB in anticipation of the added capacity needs.

Once the temporary directory has been created, the Installation Manager source files will be replicated to that endpoint. The **"creates:"** statement instructs Ansible to ignore the file upload request if the data is already replicated (this will be useful on repeat executions of the Playbook, it Ansible will not attempt to duplicate the download on subsequent runs). The third job will execute the Installation Manager and accept the end-user license for use of the product.

!!! tip "main.yml"
    Technical Sellers: As you are defining the file from scratch, your YML file will mirror the template provided below.
    Sellers: Since you are working from a Git-cloned repository, there will be additional tasks and lines of code beyond what is shown below — ignore those for now.

{% raw %}
```
---
- name: Creating staging directory
  file:
    path: /tmp/im
    state: directory

- name: copying IM source files
  unarchive:
    src: /files/aix/websphere/InstallationManager/1.8.9.4/aix.gtk.ppc_1.8.9004.20190423_2015.zip
    dest: /tmp/im
    creates: /tmp/im/userinstc.ini

- name: installing installation manager
  shell: /tmp/im/installc -log /tmp/im.lof -acceptLicense

```
{% endraw %}

!!! note "Notice to Technical Sellers"
    If you wish, you can test the Roles defined so far by executing the Playbook. You can repeat this step following every modification to the *main.yml* manifest. Ansible will take note of the additions made to the job sequence, repeating steps if necessary but avoiding redundant work (if a job has previously been executed). By the conclusion of this lab, all of the jobs necessary to fully install and deploy WebSphere Application Server will be in place and have been executed. If you are a SELLER (or are working with the Git-cloned template), execution of the Playbook will result in the full end-to-end deployment of WAS.
    The Playbook can be executed at any time using the following instruction:
    ```
    ansible-playbook was.yml -v
    ```

When satisfied, press *ESC* and then *:x* and *Return* to save and exit.
